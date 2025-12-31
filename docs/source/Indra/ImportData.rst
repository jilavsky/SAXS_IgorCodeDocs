.. _import_data_procedure:
.. _import_data_panel:


Import USAXS/SAXS/WAXS data in Igor Pro
=======================================


.. index::
    Import USAXS/SAXS/WAXS data

**Comments:**

Importing tool uses "USAXS" naming system for USAXS and "QRS" for SAXS and WAXS. When you use Irena to analyze, plot, or even only export data, you need to select "USAXS" or "QRS" choice in the top of the panels.


.. index::
    Collected data arrangement 2

Collected data arrangement
--------------------------

When you collect data on 12IDC USAXS/SAXS/WAXS instrument, your data are saved in your MM_DD_userName folder. The folder name is created by adding \MM_DD_ (\month_day_) to the name staff selects, typically version of PI user name. This user folder has internal folder structure with \"SampleName" leve of folders. These folders can be created by users (Command is : RE(newSample("sampleName"))). Note, that the default sampleName is "data" which is used when a newUser command is run.  
When you collect USAXS data, a folder with the same name as sample name but with appended "_usaxs" is created inside the sampleName folder. For SAXS data we create folder with the same name with "_saxs" and for  WAXS with "_waxs". See below in the Image:

.. Figure:: media/USAXSComputerDataArrangement.jpg
        :align: center
        :width: 280px

After you run your USAXS/SAXS/WAXS experiment, you will have somehow available a MM_DD_userName folder. Typically users will download the folder on USB drive or we will share the folder with users in Box.com (ANL cloud provider). Typical size is less than 4MB of data per USAXS/SAXS/WAXS set for each sample. Note, that typically we also collect jpg image of each sample before each data collection (small, about 100kb images) as "witness" images. Instrument truns automatic data reduction on each data set - for details see automatic data reduction description :ref:`Matilda Automatic data reduction <automatic_data_reduction>`.  


Indra release 2.05 adds new tool which can (currently) import easily and quickly all data from folders of these Nexus - NXcanSAS - data files. For now this code is in beta, but it has been used in 2025-03 by users at the beamline and seemed to work fine. For now the code is mostly useful as import tool, but it cna also re-reduce the data with simple default parameters. I plan to update the code to to have more features for data reduction over time. 


To Open the new GUI, use the menu **"USAXS-SAXS-WAXS data reduction (new)"** and you will get a new panel:


.. Figure:: media/USAXS_new_1.jpg
        :align: center
        :width: 280px



.. Figure:: media/USAXS_new_2.jpg
        :align: center
        :width: 480px


There are two main buttons which can be used to Import the data. 

Import Whole Sample
-------------------

In this case everything in a sampleFolder subfolders will be imported. This is more or less "One button" solution. Push this button, select "sampleName" folder and code will import all existing USAXS, SAXS, and WAXS data from subfolders. Images are not imported. This is the fastest way to get data in Igor Pro. The image below is of data imported this way - selecting folder "BelowEdge", code imported all data from subfolder "BelowEdge_usaxs", "BelowEdge_saxs", "BelowEdge_waxs". 


.. Figure:: media/USAXS_new_3.jpg
        :align: center
        :width: 680px



Import Selected Data 
--------------------

When you need to import only some of the data, for example during data collection you need to import first few finished scans, of you have only few scans in one very large sampleFolder (why did you NOT split this in smaller chunks?)., you can follow selective procedure:

1.  use button "Select data path" and point your GUI on any one of the three subfolders. Typically the best choice is "_usaxs" folder, but it may be the other ones, if those contain more data sets. For example, if you did not collect always all three segments and one fo the folders contains more datasets than others, you need to point on the folder with most data. 

2.  Select checboxes for which segments you want to import. "USAXS?", "SAXS?", "WAXS?", "Image?" - selecting any of these (see figure above) will select these for import. 

3. Push the "Import Selected data to Igor". Note, that if the data do not exist, no error is generated and missing data are siletnly skipped. 

Below is image which shows what happens when I selected all checkboxes, including Image, and imported just one of the data sets.  

.. Figure:: media/USAXS_new_4.jpg
        :align: center
        :width: 680px

**This is BETA version** for now. Do not use the "Re-reduce data" option for now, it may nto work as expected and is work in progress. Do not expect more than what is described above for now. If you run into troubles, let me know and I will investigate. 