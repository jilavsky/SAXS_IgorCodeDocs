.. _nika-extending:
.. _ExtendingNika:
.. _LookupFunctions:

.. index:: Nika; Extend lookup functions

Parameters lookup functions
===========================

Nika can be extended to look up calibration and normalization parameters
automatically from various external records. This eliminates the need to
manually locate and enter these values for each image. The following examples
can be copied and modified to suit specific instruments and data formats.

.. _LookupFunctions.LookupFromExtraTextFile:

.. index::
      Nika lookup functions; Text file

Lookup from external text file
--------------------------------

This method is used when image files (such as TIFFs) do not store metadata
internally. The main disadvantage is that metadata and image files can become
separated, and text file formats often change over time, requiring the lookup
code to be updated accordingly. Despite these limitations, this is a widely
used approach and needs to be supported.

Two common metadata file arrangements exist:

1. **One large text file per folder** — A single text file in the same folder
   as the images (or in its parent folder), containing one line of information
   per image, usually including the image filename in each line.

2. **One text file per image or group of images** — Each image (or group of
   images) has an associated text file with a related name. The text file may
   contain header lines followed by data lines, one per image.

For the first arrangement, the most efficient approach is to load the text file
into Igor waves in a separate folder and look up values from those waves. Write
a dedicated Igor function to load the file so that wave names and locations
remain consistent.

For the second arrangement, the following example function shows how to read a
specific value from a per-image text file. The setup assumed here: a folder
contains groups of image files named ``image_file_01.tif``,
``image_file_02.tif``, etc. (pattern: ``ArbitraryString_XY.tif``). Each group
has an associated text file named ``image_file.txt``
(``ArbitraryString.txt``). The text file contains 9 header lines followed by
tab-separated data lines, one per image, with columns: date, time, filename,
Monitor1, Monitor2, Thickness, Transmission.

Example data line in a text editor::

    10/04/2017 15:54:04 170410_refs_01.tif  87.259 87.145  1  45.457

Same line as seen by Igor (tabs shown as ``\t``)::

    10/04/2017 15:54:04\t170410_refs_01.tif\t87.259\t87.145\t1\t45.457;

The following function reads the sample thickness (column 5, zero-indexed as
item 4) from this text file:

.. code::

  Function ReadThickness(FileName)
       string FileName
      //This function depends on a fixed naming structure and known text file content.
      //Modify to return different values from the table as needed.
      //
      //To use: place this code in a .ipf file in
      //Documents/Wavemetrics/Igor Pro 9 Igor Procedures/
      //ipf files in that folder load automatically when Igor starts.
      //
      //Then in Nika: check "use sample Thickness (t)" checkbox and
      //on tab "Par" check "Use fnct?" for Sa Thickness; in the field
      //type ReadThickness (no quotes or parentheses, just the function name)
      //
      //This function assumes the naming template:
      //  image file: imageName_XX.tif  ->  text file: imageName.txt
      //Text file content line example:
      //  date time\tFileName\tMon1\tMon2\tThickness\tTransmission[%]
      //  10/04/2017 15:54:04\t170410_refs_01.tif\t87.259\t87.145\t1\t45.457
      //Returns the 5th element (thickness, item 4 in zero-based indexing)
      //
          // Check that the path to 2D data exists (Nika's symbolic path)
      PathInfo Convert2Dto1DDataPath
      if(V_Flag<1)
        Abort "Path to 2D data does not exist"
      endif
          // Determine the text file name from the image file name
      string TextFileName
      variable NumOfSeparators
      NumOfSeparators = ItemsInList(FileName, "_")
            // Assume the last "_"-separated segment is the XX.tif part
      string EndStuff=StringFromList(NumOfSeparators-1, FileName, "_")
      TextFileName = ReplaceString(EndStuff, FileName, "")
      TextFileName = removeEnding(TextFileName,"_")
      TextFileName = TextFileName+".txt"
          // Open the file as read-only (must be closed when done)
      variable i, refNum, matched
      string aLine
      Open /P=Convert2Dto1DDataPath /R /T=".txt" refNum  as TextFileName
          // Skip the 9 header lines
      For(i=0;i<10;i+=1)
        FreadLine refNum, aLine
      endfor
          // Read subsequent lines until the one containing this image filename is found
      Do
        i+=1
        FreadLine refNum, aLine
        if(strlen(aline)<1)
          Abort "Data for the image name "+FileName+" was not found in the text file."
        endif
        if(GrepString(aLine, FileName ))
          matched=1
          endif
      while(!matched)
      close refNum              // Important: always close the file
          // aLine now contains the data line for this image
          // Replace tabs with semicolons for use with StringFromList
      aLine=ReplaceString("\t", aLine, ";")+";"
          // Extract thickness (item 4, zero-based); Nika expects thickness in [mm]
      variable result
      result = str2num(StringFromList(4, aline, ";"))
        // Note: this approach is practical for up to a few hundred lines.
        // For thousands of images, load the full text file into Igor waves first
        // and look up values from those waves instead.
      return result
  end


.. _LookupFunctions.LookupFromWaveNote:

.. index::
    Nika lookup functions; Metadata
    Nika lookup functions; Wave notes

Lookup from wave note metadata
--------------------------------

When Nika loads an image with embedded metadata — such as HDF5/NeXus files
(see :ref:`Nexus <Nexus>`) — it appends the metadata to the image as a wave
note in ``Keyword=Value;`` format::

    KeyWord1=Value1;KeyWord2=Value2;...

Knowing the keyword names for the values of interest makes lookup
straightforward using Igor's ``NumberByKey`` function.

Useful wave paths:

* Current 2D image: ``root:Packages:Convert2Dto1D:CCDimageToConvert``
* Current 2D empty: ``root:Packages:Convert2Dto1D:EmptyData``
* Current 2D dark: ``root:Packages:Convert2Dto1D:DarkFieldData``

The following function reads an ion chamber count value from the wave note for
normalization purposes. A similar approach can extract photodiode and ion
chamber readings from both sample and blank images to calculate per-sample
transmission automatically.

.. code::

  Function FindI0(SampleName)
    string sampleName
    Wave/Z w2D = root:Packages:Convert2Dto1D:CCDimageToConvert
    if(!WaveExists(w2D))
        Abort "image file not found"
    endif
    string OldNOte=note(w2D)
    //OldNOte should have data like this: ...;I0_cts=56.5;I0_gain=1000000;...
    variable I0 = NumberByKey("I0_cts", OldNote  , "=" , ";")
    variable I0gain = NumberByKey("I0_gain", OldNote  , "=" , ";")
    I0 = I0 / I0gain
    //check for NaN result
    if(numtype(I0)!=0)
        //abort "Nan result found"     //uncomment to abort on failure
        Print "I0 or I0gain value not found, setting to 1"
        I0=1
    endif
    return I0
  end
