.. _import_data:

.. index::
    Import data

Import data
===========

This chapter describes how to import various types of 1D SAS and WAXS data in Irena. Irena has various importers - for ASCII, HDF5 canSAS Nexus, & canSAS XML. *Users of Indra 2 or Nika produced data can skip this chapter.*

I have included ASCII set of data from our USAXS experiment as example on which user can play and test capabilities, they can be found where the Irena macros are installed, typically Documents/Wavemetrics/Igor Pro 7 folder/User Procedures/Irena/Example Data/.

**Comments:**

When loading data for use with Irena macros, the user needs to decide ahead on naming system, which will be used. Unless you have USAXS data, do not use USAXS option, this IS NOT correct choice for users from other instruments. *Use “qrs” naming convention, where the wave with q vector starts with q\_\ andTheDataName, intensity wave is named r\_\ andTheDataName, and error wave s\_\ andTheDataName. This allows placing multiple data sets in one Igor folder, which I strongly discourage*. Note, that when Irena macros save results within Igor experiment, they **ASSUME**, that they can write simply solution into folder where the SAS data came from. Irena macros **WILL NOT** overwrite old results, but it may be impossible to figure out, which data the particular result belongs to…. To make best use of these macros, please use folder structure with folder names being the sample name.

**YOU WERE WARNED!!!!**

Description
-----------

.. Figure:: media/ImportData1.png
        :align: center
        :width: 280px

In “Data import & export” from “SAS” menu select either:

-  “Import ASCII data”

-  “Import ASCII WAXS or other data”

-  “Import Nexus canSAS data”.

-  “Import canSAS XML data”.

Each tool has its own screen and there are small differences in operations. This chapter described ASCII importer in details and for the others only the differences. So read the whole thing!

NOTE: you can have ONLY one of the importers opened at any given time, they share same working folder and when you try to open any one of these importers, it should force closing of any other opened panel for the other importers.

.. index::
    Import data; ASCII SAXS

Importing ASCII SAS data
------------------------

.. Figure:: media/ImportData2.png
        :align: left
        :width: 300px
        :Figwidth: 320px

Explanation of control available here:

“\ *Select data path”* browse to the folder on the computer drive where the data to be imported reside.

“\ *Data path”* this shows the path selected above. Cannot be edited in this window, use button *Select data path*.

“\ *List of available files”* lists all files in the current folder on the computer, unless masked by *Data extension*. One or more files here can be selected for import. Use shift - click to select multiple files (on Windows) or cmd – click on Macs (to pick one file at time), shift-click to pick range of files. Double click on file runs "Test" and "Preview" commands on that file.

*"Match name"* enables to use string to select subset of files.

“\ *Data extension”* if extension is put in this filed (e.g., “dat”) only files with the “dat” extension will be shown in the *List of available files*.

*“Skip lines”* if there are known number of lines, which need to be skipped. Note, Igor will automatically read file structure and skip usually any text header, which needs to be skipped. Therefore this “skip lines” should not be usually necessary.

*“Test”* Test import of first selected file. Not really necessary, but very useful. Sets checkboxes for Column 1 to 6, how many columns were found in the file, etc.

*Red text indicating too many data points* - lot of data from SAXS instruments contains very high number of data points which are really useless for SAXS data analysis. Actually, they are bad, as they force code to fit too many noisy points. This warning comes when too many points are found. See below the controls for reduction of data points.

*“Preview”* Opens the first selected file in Igor notebook for preview. Kill notebook after use, it is not needed for anything else…

*“column 1 – 6”* and *Qvec Int err*\ ” This is checkbox area, in which user needs to select which column of data contains which SAS data. Assumption is, that SAS data are in the first 6 columns in the ASCII file. These checkboxes appear when “\ *Found columns”* number gets set. User can set it or it gets set during “\ *test*\ ”.

”\ *Select all”* or “\ *Deselect all”* modifies which files are selected in “\ *List of available files”*.

*“Qvec units”* select proper checkbox. Units will be converted to A\ :sup:`-1` if nm\ :sup:`-1` data are imported. Irena uses A\ :sup:`-1`.

“\ *Create errors”* if the data imported do not contain error bars, this will generate sqrt(Intensity) error bars. These can be further modified (multiplied) in Data manipulation tool.

“\ *UTF-8"* if Igor cannot recognize ASCII file encoding, it will present dialog which require user to select encoding for EACH file being imported. This can get pretty annoying and happens when the ASCII header contains non-standard ASCII character (in example I have greek letter denoting micrometer). In this case you can check this checkbox \"UTF-8?\" encoding and file will be imported forcefully as if it is UTF-8. This may cause issues, but unlikely. Default is unchecked.


“\ *Use file name as folder name”* **Strongly suggested to use**. Will cause the import tool to create for each imported data set new folder with name by the file name.

“\ *Use USAXS names”,* ”\ *Use qrs wave names”, "Use QIS (NIST) wv nms"* selects which naming structure is used during import of data. One of these selections is more or less necessary for multiple file import.

*"Auto overwrite"* Overwrites existing folders in same named data are imported second time.

**Following modifications of data are done in this order, if selected…**

*(Q units conversion to A)*

*“Scale imported data?”* if the data need to be scaled by some calibration factor… New input variable appears, if necessary.

*“Slit Smear imported data?”* if the data need to be slit smeared… New input variable appears, if necessary. This is useful when pinhole data need to be smeared for use with USAXS/USANS data. Use Slit length in Q units [A\ :sup:`-1`]. Even if you have data in nm\ :sup:`-1` since the conversion to A is done first. NOTE: if you provide dq data (q-resolution) these will be for slit smeared data convoluted with the SlitLength. If you do not provide these data, new dQ wave will be created with Slit length assigned to each point as resolution.

*"Remove Int<=0"* removes any negative (or equal 0) intensities during import.

*"Trim data"* opens two new input variables and enables to trim Q range of data being imported. 0 means no trimming in that "direction". Otherwise, input Qmin or Qmax as needed.

*"Reduce data points"* reduces number of points by averaging on log-scale. Suggested for data with large number of points at high Q (if more than 250 points is found, warning appears below "test" and "Preview" buttons. Note, this step creates new Q resolution wave - even though currently Irena is not using Q resolution data for anything.

*"Truncate start/end of long names"* - allows users to choose how to truncate long names (current limit is 26 characters which user can use). Important if the "important" part of the name is at the end...

*"Remove Str From Name ="* - allows users to remove part of the sample name to get the useful information into the limit of 26 characters which user can use. Important if the "important" part of the name is at the end...

Note: from version 2.51 I have added another row of checkboxes to include in the wave note of the Intensity Units. In the future this will be used by other Irena code:

*"Calibration Arbitrary"* *"Calibration cm2/cm3"* *"Calibration cm2/g"* - Irena always assumed standard cm2/cm3 calibration of the intensity data and then provided results on absolute scale. By selecting correct calibration method the tools (as of 2.53 Modeling II and Plotting tool I) will be aware of calibration string and provide proper units to output data. Of course, even if data are on absolute scale if you do not provide correct contrasts for analysis, results cannot be on absolute scale and Irena has no way of knowing it.

Single file import can be done by manually filling the following controls.

“\ *Select data folder”* and “\ *New data folder”* Using pull-down menu in *Select data folder* user can select existing data folder where to put the imported data. Using *New data folder* user can create folder in Igor for the data. Note, that “<filename>” will be replaced with the file name of the imported data file during import. This allows for creating data structure which uses folders during multiple file import.

“\ *Intensity wv name”*, “\ *Q wave name”*, and “\ *Error wave name”* – these can be filled with the names for data waves. Note, that “<filename>” will be replaced with the file name of the imported data file during import.

“\ ***Import”*** imports the selected data.

NOTE: If the data contain header of data (typically number of lines with special character, such as #, $, ... at the start of the line and some spaces before useful information, Irena ASCII importer will attach these notes into the wave note. It will, however, first remove all special characters and spaces from the beginning of each line. The code will search each line for first character, which is letter or number and then accept the rest of the line. It will remove any line-feed and/or carriage returns at the end of each line. It will separate lines in the wave note by using ";" character.

Some of the controls (checkboxes) do change some of the setting in other controls. Generally the proper order, how to select and modify control is from top to bottom.

.. index::
    Import data; ASCII WAXS or other

Importing ASCII WAXS data
--------------------------

.. Figure:: media/ImportData3.png
        :align: center
        :width: 380px

This tool is intended for other type of data, such as powder diffraction, which have x-axis, Intensity, Uncertainty and, optionally, x-resolution in ASCII file. Options here are bit more limited to only those, which seemed important for this purpose.

This was added for users of non-SAS data who had problems using the original ASCII imported since it was doing things not appropriate for heir data.

.. index::
    Import data; canSAS Nexus

Importing Nexus canSAS data
---------------------------

.. Figure:: media/ImportData4.png
        :align: center
        :width: 380px

**What is Nexus and why do I care???**

Nexus is attempt of X-ray and Neutron (or likely Neutron and X-ray) communities to develop file format, which can be used to share and store data from X-ray and Neutron instrument in such way, that they are generally readable and usable. The file system uses HDF5 file format – this is binary container for data (similar to xls Excel format, pxp Igor format etc.) HDF5 is supported by many commercial packages and it support is available for most programming environments. It is free to use and well maintained. Simply put, HDF5 is useful form of storing data.

Nexus provides description of how to store data and what to store – how to call various data (e.g., use “wavelength”) etc. For most of you this is useless information.

**Why you want to use it?** – By having definition of what and where to expect, any program supporting specific Nexus class should be able to read your data. This should enable our user community to exchange data easily between instrument, data reduction package, and data analysis package.

Where are details?

http://www.nexusformat.org

http://download.nexusformat.org/doc/html/index.html

**More to know:**

Irena supports only one of two “classes” or “Application definitions” important for its users case 2 in the list below:

1. input of raw data from instruments, follows “NXsas” application    definition.

2. output of reduced (1D or 2D) data for analysis software (“NXcanSAS”)

Theoretically it is possible to store both in the same Nexus file. My program Nika for now (version 1.75) creates two files. Single file can be implemented easily, if anyone needs it.

**In summary**: If you are lucky enough and have data in Nexus format, various packages should be able to read the data with minimum problems. Nexus is very flexible. canSAS working group of small-angle scatterers – typically instrument scientists at large facilities – developed canSAS specifications as “application definition”, which are intended for 1D and 2D reduced SAS data (X-ray or Neutrons). Starting version 2.62 Irena can import 1D canSAS Nexus data. And Nika released at the same time can export 1D canSAS data.

Note, that there are very few controls in the GUI for this tool as there should not be many decisions to be made. You may test what to use for naming of the Igor folders. If the file has poorly named entries, you can overwrite previously imported data, so be careful about importing. This tool overwrites data.

If you need to peek inside the file to see what is inside, select it, push “Open File in Browser” and Igor HDF5 Browser is used to open the file, so you can look inside it.

If you are missing data after import or foldernames make no sense, try using different “Use … as Fldr Nms”.

If all fails, send me the file and I’ll see if and how I can help.

Keep in mind, that as every standard made by committee canSAS nexus is way too flexible for its own good and weird stuff happens. And not every file really follows required and suggested Nexus structure.


.. index::
    Import data; canSAS XML

Importing XML data
------------------

.. Figure:: media/ImportData5.png
        :align: center
        :width: 380px


NOTE: XML data tool requires xop for XML data file interface. See chapter 0.4 above for the link to this file.

Similar controls, except canSAS XML file does not need some of the controls. Therefore, the GUI can be easier. On the other hand there may be more data columns (meaningful) in this data file and while Irena does not use any of these, they can be loaded to be useful for user code or other tools, which may be able to use them (like NIST macros).

If anyone has actually real world example of canSAS xml data, can you send me and example, please?


Walk through Importing test file
--------------------------------

Using *Select data path* button select folder on the computer, where Irena data are installed, for example:

.. Figure:: media/ImportData6.png
        :align: center
        :width: 400px


and in *Data extension* input “dat”. The following should be the panel:

.. Figure:: media/ImportData7.png
        :align: center
        :width: 380px

Select the “Test data.dat” file and double click - or push *Test* and *Preview* buttons.

.. Figure:: media/ImportData8.png
        :align: center
        :width: 680px

Igor found 3 columns of data so 3 rows of checkboxes appeared. The *Preview* has created notebook on right, where user can preview the file and check, which columns contain which data. Note, that Igor skipped the block of text in the beginning of the data file automatically.

Check cheboxes according to following screen and noticed, that *Create errors* checkbox becomes unavailable when any checkbox in the Err column is selected. Notice, that when checkboxes *Use file nms as Fldr Nms* and *Use QRS wave names* are checked, the names for folder and data wave names are filled in with default.

.. Figure:: media/ImportData9.png
        :align: center
        :width: 380px


Now push *Import* and the data are imported. Kill the Import data panel and see in Data browser:

.. Figure:: media/ImportData10.png
        :align: center
        :width: 680px


Here is bit more complicated example:

.. Figure:: media/ImportData11.png
        :align: center
        :width: 680px


Note: I have selected may be 136 data sets here, I have decided to trim data (note in the notebook that there are no data bellow Q of 0.006) I have also reduced number of points to 200 from 861, limited high q range (no data found above Q of 0.85) and removed negative intensities. This load creates much more easy to handle data with q scale logarithmic and not linear with less noise at high q, which is much easier to plot and analyze.
