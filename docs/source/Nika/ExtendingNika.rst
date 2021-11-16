.. _ExtendingNika:

.. _LookupFunctions:

.. index:: Extend Nika Lookup functions


Parameters lookup functions
===========================

Nika can be extended and customized to lookup parameters from various records. This enables users to have to manually find normalization and calibration numbers in their notebooks or other records. Following are some examples of these functions, which users can copy and modify to suit their needs.


.. _LookupFunctions.LookupFromExtraTextFile:

.. index::
      Nika Lookup Functions; Text file

Lookup from external text file
------------------------------

This is an ugly (obsolete) method which has been used for long time with images (like tiff files) which do not easily enable storing metadata with the image itself. The main disadvantage of this method is a chance that metadata and image get separated and generally it is cumbersome. Also, usually this method is created ad hoc and also ad hoc modified. This makes it for very poor target for programming, as the code which handles such metadata is always changing. But, this is widely used, common method, and therefore needs to be supported.

There are two basic methods of how metadata are stored - at least from my experience. I used both of these methods long time ago and still see many users with data stored this way.

1.  One (potentially very large) text file in a folder with lots of images (or in the parent folder of the folder with these images). This text file contains one line of information for each image in that folder, usually with file name of the image within the line itself.

2.  Many (smaller) text files, where each image (or few images) have one associated text file - usually with name related to the images name by simple recipy. This text file can now contain some header info and possibly, like in the case example below, smaller number of lines where each line relates to one of the images.

For the one large file the best method is to manually load the text content in Igor first in separate folder in waves and then look up these values from there. This can be done relatively efficiently and one can (and should) write Igor function which loads the text file so wave names and location of the waves are fixed and code then can rely on their presence.

Recently I was asked to help users with the second setup - one text file for few images - to write look up functions. Below is an example of the resulting functions which you can modify with relatively little Igor expertise...

In this case we have a folder contains multiple "groups" of files belonging together. Each group has some number of images with names similar to image_file_01.tif, image_file_02.tif, etc. Names are basically "ArbitraryString_XY.tif". Then we have associated text file, in this case named image_file.txt (that is "ArbitraryString.txt"). Inside the text file are few (9 in my case) lines of header with various common information (sample name, instrument conditions, etc.) and then lines with tab separated values for each image - such as date+time, file name, Monitor1, Monitor2, thickness, transmission etc.

In text editor or Igor history area this line looks like this:
10/04/2017 15:54:04 170410_refs_01.tif  87.259 87.145  1  45.457

In Igor debugger (and for Igor code) this line looks like this:
10/04/2017 15:54:04\\t170410_refs_01.tif\\t87.259\\t87.145\\t1\\t45.457;
note that the  "\\t"  is Igor for tab.

To read a specific value from this code I wrote following function:


.. code::

  Function ReadThickness(FileName)
	   string FileName
      //if you need help with your function, make sure you send me your current version
      //and include data and how it fails.
      //This function depends on rigid name structure and known and fixed text file content.
      //this function can be easily modified to return different values from the table
      //
      //To use this place this code in a text file with extension .ipf file in
      //Documents/Wavemetrics/Igor Pro 8 User Procedures/Igor Procedures
      //(or /Igor Pro 9 Igor Procedures/)
      //ipf files placed in Igor Procedures folder are loaded always when Igor starts.
      //
      //Then in Nika : check "use sample Thickness (t)" checkbox and
      //on tab "Par" check "Use fnct?" for Sa Thickness in the field
      //type ReadThickness    (no " or ' , just the letters) with no parameters or ()
      //
      //this function reads from text file assuming name template:
      //image in name: imageName_XX.tif ------> the text file name:  imageName.txt
      //text file content line example:
      //date time\tFileName\tMon1\tMon2\tThickness\tTransmission[%]
      //10/04/2017 15:54:04\t170410_refs_01.tif\t87.259\t87.145\t1\t45.457
      //we need to return 5th element - thickness - (Igor is 0 based, so item 4)
      //
          // ----- here we go -----
          //First check if path to data exists, this is symbolic path Nika uses to name
          //location of the 2D data. Basically, images are here...
      PathInfo Convert2Dto1DDataPath
      if(V_Flag<1)					             //path does not exist
        Abort "Path to 2D data does not exist"	 //abort
      endif
          //now image out the name of the text file. The depends on naming system
      string TextFileName                 //place for the file name
      variable NumOfSeparators            //need a number
      NumOfSeparators = ItemsInList(FileName, "_")	//number of string parts separated by "_"
            //"_" is common seprator for user name parts, so we need to assume more than one.
            //Assume the last one is the XX.tif part
      string EndStuff=StringFromList(NumOfSeparators-1, FileName, "_")
                                                             //this is the XX.tif part
      TextFileName = ReplaceString(EndStuff, FileName, "")	 //remove the XX.tif part
      TextFileName = removeEnding(TextFileName,"_")		     //remove "_"
      TextFileName = TextFileName+".txt"	                 //add .txt to the name
                                      //this should be the text file name now.
      //print TextFileName            //for testing purposes
          //now we can open the file and read it line by line.
          //This can be done more efficiently, but if this file is not too long,
          //we can simply read through this line by line. Makes it easier to understand...
      variable i, refNum, matched
      string aLine
          //Open the file as read only.
          //We need to eventually close it so it does not stay open!
      Open /P=Convert2Dto1DDataPath /R /T=".txt" refNum  as TextFileName
          //iterate through first 9 lines
          //in my case example each text file had 9 lines of header
          //and then one line per file info
      For(i=0;i<10;i+=1)		                    //9 lines of header info
        FreadLine refNum, aLine
        //print aLine		                         //for testing
      endfor
          //now we need to read and check each following line until
          //we find the one with the right file name in it...
      Do			            //this loop could be done better
                          //but this should be easier to understand and modify.
        i+=1			                        //line number, increment by +1
        FreadLine refNum, aLine		//read the line
        if(strlen(aline)<1)		//if aLine is empty we are the end of
                              //this file, Abort, did not find line which we needed...
          Abort "Date for the image name "+FileName+" was not found in the text file."
        endif
        if(GrepString(aLine, FileName ))	//check if it contains file name
          matched=1                         //if yes, we have our line
          endif
      while(!matched)           //if matched, we can continue with this line
                                //else back in the loop...
      close refNum              //important, close the file.
          //now we have in string "aLine" the line from text file which
          //contains the name of the file we are dealing with...
      //print aLine						        //for testing
          //note, in my case aLine is separated by tabs = '\t'
          //let's clean it up a bit,
      aLine=ReplaceString("\t", aLine, ";")+";"
                  //replace '\t' with ; and append ; at the end...
      //print aLine						        //for testing
          //now we need to find the right number and return it to Nika...
      variable result
          //Now it depends, which item is what.
          //Assume Thickness is fifth item (item 4, Igor is 0 based), for example...
          //Note: Nika expects thickness in [mm]
      //print str2num(StringFromList(4, aline, ";"))
      result = str2num(StringFromList(4, aline, ";"))			//thickness [mm]
        //done, result has value we wanted...
        //This will work for reasonable number of lines/images in the text file listing
        //(I guess up to hundred), will get really slow for large number (thousands) of lines/images.
        //If large number of images (=lines) is in the text file, the only efficient way
        //is to load such large list in Igor first in separate folder in waves
        //and then look up in these waves - that avoids reading many times line by line from a
        //text file. Can be done, but would be two step procedure.
      return result
  end


.. _LookupFunctions.LookupFromWaveNote:

.. index::
    Nika Lookup Functions; Metadata
    Nika Lookup Functions; Wave notes

Lookup from wavenote metadata
-----------------------------

When Nika loads image with metadata - like the HDF5 images :ref:`Nexus <Nexus>` it appends the metadata information to image as wave note. It creates first from the metadata keyword=Value; string (KeyWord1=Value1;KeyWord2=Value2;...) so this info can be easily searched. You need to know the Keywords, of course, but then this is very easy to look up and calculate what is needed...

Helpful notes:
  Current 2D image ...   root:Packages:Convert2Dto1D:CCDimageToConvert

  Current 2D Empty ...   root:Packages:Convert2Dto1D:EmptyData

  Current 2D Dark  ...   root:Packages:Convert2Dto1D:DarkFieldData

Following is example which my instrument uses to look up Ion chamber counts collected during exposure for normalization purposes. Similar code can be used to extract photodiode and ion chamber counts measured during transmission measurements on sample and empty (blank) image - and calculate transmission of each sample "on fly".

.. code::

  Function FindI0(SampleName)
    string sampleName
    Wave/Z w2D = root:Packages:Convert2Dto1D:CCDimageToConvert //this is actually the current image
    if(!WaveExists(w2D))
        Abort "image file not found"   //error message to user, this should not happen.
    endif
    string OldNOte=note(w2D)
    //OldNOte should have data like this ...;I0_cts=56.5;I0_gain=1000000;...
    variable I0 = NumberByKey("I0_cts", OldNote  , "=" , ";")
    variable I0gain = NumberByKey("I0_gain", OldNote  , "=" , ";")
    //print SampleName+"   normalized I0 = "+num2str(I0 / I0gain)
    I0 = I0 / I0gain
    //check for failure when we get for some reason NaN (not a number)
    if(numtype(I0)!=0)
        //abort "Nan result found"     //you can abort if needed.
        //or simply overwrite this to 1 and report to user.
        Print "I0 or I0gain value not found, setting to 1"
        I0=1
    endif
    return I0
  end
