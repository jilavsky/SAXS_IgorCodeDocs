.. _ExtendingNika:

.. _LookupFunctions:

.. index:: Extend Nika Functions


Parameters lookup functions
===========================

Nika can be extended and customized to lookup parameters from various records. This enables users to have to manually find normalization and calibration numbers in their notebooks or other records. Following are some examples of these functions, which users can copy and modify to suit their needs.


.. _LookupFunctions.LookupFromExtraTextFile:

.. index::
   Lookup Functions Nika; Text file

Lookup from external text file
------------------------------

This is an ugly workaround which has been used for long time with images (like tiff files) which do not easily enable storing metadata with the image itself. The main disadvantage of this method is a chance that metadata and image get separated. Also, usually this method is created ad hoc and also ad hoc modified. This makes it for very poor target for programming, as the code which handles such metadata is always changing. But, this is very common method and therefore needs to be supported.

There are two basic methods of how metadata are stored - at least form what I have seen. Potentiallt large text file in a folder with images (or just above the folder with these images) which contains one line of information for all images in that folder. And case where each (or few) images have associated text file (of similar anme to images names) containing smaller number of lines.

For the one large file the best method is to load the text content in Igor first in separate folder in waves and then look up these values from there. This can be done relatively efficiently.

Recently I was asked to help users with the second setup to write such functions. Here is an example of the resulting functions...

In this case we have a folder conating multiple "groups" of files belonging together. Each group has Number of Images with names similar to Image_file_01.tif, Image_file_02.tif, etc. Names are bascially ArbitraryString_XY.tif. Then we have associated text file, in this case names Image_file_refs.txt (that is ArbitraryString_refs.txt). Inside the test file are few lines of header with various other information (sample name, instrument conditions, etc.) and then lines with tab separated values such as date+time, Image_file_XY.tif, Monitor1, Monitor2, thickness, transmisison etc. In Igor this line looks like this:
10/04/2017 15:54:04/t170410_refs_01.tif/t87.259/t87.145/t1/t45.457
note that the  /t  is Igor for Tab.

To read a specific value from this code I wrote following function:


.. code::

  Function ReadThickness(FileName)
	   string FileName
      //if you need help with your function, make sure you send me your current version and include data and how it fails.
      //This function depends on rigid name structure and known and fixed text file content.
      //this function can be easily modified to return different values from the table
      //
      //To use this place this code in a text file with extension .ipf file in
      //Documents/Wavemetrics/Igor Pro 7 User Procedures/Igor Procedures (or /Igor Pro 6 Igor Procedures/)
      //ipf files placed in Igor Procedures folder are loaded always when Igor starts.
      //
      //Then in Nika : check "use sample Transmission (T)" checkbox and
      //on tab "Par" check "Use fnct?" for Sa Transmisison in the field
      //type ReadTransmission    (no " or ' , just the letters) with no parameters or ()
      //
      //this function reads from text file assuming name template:
      //image in name: ImageName_XX.tif ------> the text file name:  ImageName.txt
      //text file content line example:
      //10/04/2017 15:54:04/t170410_refs_01.tif/t87.259/t87.145/t1/t45.457
      //we need to return 5th element - thickness - (Igor is 0 based, so item 4)
      //
          // ----- here we go -----
          //First check if path to data exists, this is symbolic path Nika uses to name location of the 2D data. Basically, images are here...
      PathInfo Convert2Dto1DDataPath
      if(V_Flag<1)					             //path does not exist - something bad happened...
        Abort "Path to 2D data does not exist"	 //abort so user knows bad things happened.
      endif
          //now figure out the name of the text file. The depends on naming system
      string TextFileName                 //place for the file name
      variable NumOfSeparators            //need a number
      NumOfSeparators = ItemsInList(FileName, "_")	//number of string parts separated by "_"
            //"_" is common seprator for user name parts, so we need to assume more than one.
            //Assume the last one is the XX.tif part
      string EndStuff=StringFromList(NumOfSeparators-1, FileName, "_") //this is the XX.tif part
      TextFileName = ReplaceString(EndStuff, FileName, "")	 //remove the XX.tif part
      TextFileName = removeEnding(TextFileName,"_")		     //remove "_", it needs to be removed
      TextFileName = TextFileName+".txt"	                 //add .txt to the name, this should be the text file name now.
      //print TextFileName						             //for testing purposes
          //now we can open the file and read it line by line.
          //This can be done more efficiently, but if this file is not too long,
          //we can simply read through this line by line. Makes it easier to understand...
      variable i, refNum, matched
      string aLine
          //Open the file as read only.
          //We need to eventually close it so it does not stay open!
      Open /P=Convert2Dto1DDataPath /R /T=".txt" refNum  as TextFileName
          //iterate through first 9 lines - in my case example each text file had 9 lines of header and then one lin eper file info
      For(i=0;i<10;i+=1)		                    //9 lines of header info
        FreadLine refNum, aLine
        //print aLine		                         //for testing
      endfor
          //now we need to read and check each following line until we find the one with the right file name in it...
      Do			                            //this loop could be done better, but this should be easier to understand and modify.
        i+=1			                        //line number, increment by +1
        FreadLine refNum, aLine					//read the line
        if(strlen(aline)<1)						//if aLine is empty we are the end of this file, Abort, did not find line which we needed...
          Abort "Info for the file name "+FileName+" was not found in the text file. Something is wrong here"
        endif
        if(GrepString(aLine, FileName ))		//check if it contains file name
          matched=1								//if yes, we have our line
          endif
      while(!matched)		     				//if matched, we can continue with this line, else back in the loop...
      close refNum						    	//important, close the file.
          //now we have in string "aLine" the line from text file which contains the name of the file we are dealing with...
      //print aLine						        //for testing
          //note, in my case aLine is separated by tabs = '\t'
          //let's clean it up a bit,
      aLine=ReplaceString("\t", aLine, ";")+";"	//replace '\t' with ; and add one ; at the end... Needed for lookup next
      //print aLine						        //for testing
          //so now we need to simply find the right number and return...
      variable result
          //Now it depends, which item is what. Assume Thickness is fifth item (item 4, Nika is 0 based), for example...
          //Note: Nika expects thickness in [mm]
          //print str2num(StringFromList(4, aline, ";"))
      result = str2num(StringFromList(4, aline, ";"))			//thickness [mm] 	//done, result has value we wanted...
	      //This will work for reasonable number of lines/images in the text file listing (I guess up to hundred), will get really slow for large number (thousands) of lines/images.
	      //If large number of images (=lines) is in the text file, the only efficient way is to load such large list in Igor first in separate folder in waves
	      //and then look up in these waves - that avoids reading many times line by line from a text file. Can be done, but would be two step procedure.
      return result
  end


.. _LookupFunctions.LookupFromWaveNote:

.. index::
   Lookup Functions Nika; Metadata, Wave notes

Lookup from wavenote metadata
-----------------------------

When Nika loads image with metadata - like the HDF5 images :ref:`Nexus <Nexus>` it appends the metadata information to image as wave note. It creates first from the metadata keyword=Value; string (KeyWord1=Value1;KeyWord2=Value2;...) so this info can be easily searched. YOu need to know the Keywords, of course, but then this is very easy to look up and calcuate what is needed...

Helpful notes:
  Current 2D Image ...   root:Packages:Convert2Dto1D:CCDImageToConvert

  Current 2D Empty ...   root:Packages:Convert2Dto1D:EmptyData

  Current 2D Dark  ...   root:Packages:Convert2Dto1D:DarkFieldData

Following is example which my instrument uses to look up Ion chamber counts collected during exposure for normalization purposes. Similar code can be used to extract photodiode and ion chamber counts measured during transmission measurements on sample and empty (blank) image - and calculate transmission of each sample "on fly". 

.. code::

  Function FindI0(SampleName)
    string sampleName
    Wave/Z w2D = root:Packages:Convert2Dto1D:CCDImageToConvert //this is actually the current image
    if(!WaveExists(w2D))
        Abort "Image file not found"   //error message to user, this should not happen.
    endif
    string OldNOte=note(w2D)
    //we are looking for data like this ...;I0_cts=56.5;I0_gain=1000000;...
    variable I0 = NumberByKey("I0_cts", OldNote  , "=" , ";")
    variable I0gain = NumberByKey("I0_gain", OldNote  , "=" , ";")
    //print SampleName+"   normalized I0 = "+num2str(I0 / I0gain)
    I0 = I0 / I0gain
    if(numtype(I0)!=0)    //this is here to prevent bad failures, you can also abort if needed.
        Print "I0 or I0gain value not found in the wave note of the sample file, setting to 1"
        I0=1
    endif
	  return I0
  end
