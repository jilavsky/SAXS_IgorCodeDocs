.. _Nika.InstrumentSupport:

.. index:: Instrument support (Nika)

Instrument support
==================

Instrument support are packages of specific additions in Nika to support special instrument. Depending on the instrument, these functions add capabilities and modify settings to make support of specific instrument easy.

Currently there are these instruments supported:

1.  :ref:`APS 9ID-C USAXS/SAXS/WAXS <Nika.9IDC_Instrument>`
2.  :ref:`APS 12ID-C SAXS/WAXS <Nika.12IDC_Instrument>`
3.  :ref:`APS 12ID-B SAXS/WAXS <Nika.12IDB_Instrument>`
4.  :ref:`ALS RSoXS soft energy SAXS <Nika.ALS_RSoXS>`
5.  :ref:`SSRL Mat SAXS <Nika.SSRL_MatSAXS>`
6.  :ref:`TPA <Nika.TPA>`
7.  :ref:`APS 5ID DND SAXS/WAXS <Nika.5ID_DND>`
8.  :ref:`SMI NSLS-II  <Nika.SMI_NSLSII>`


For other instrument scientists:
--------------------------------
Other instrument setups can be added on request. Provide me with enough data and description and I can write support for your instrument.



.. _Nika.9IDC_Instrument:

.. index::
    Nika; 9ID-C instrument support

9ID SAXS - WAXS
---------------

This is support for APS beamline 9ID SAXS and WAXS instruments. This is my beamline and there are instructions for how to use this tool. These instructions open when you select this choice. There is also Youtube movie which walks users on how to reduce their data.

.. _Nika.12IDC_Instrument:

.. index::
    Nika; 12ID-C instrument support


12ID-C SAXS using Gold detector
-------------------------------

This is support for APS beamline 12ID-C SAXS instrument. Short instructions are displayed when option is selected. Note: you need folder of tiff files, spec file and optionally also beamline data reduction script (typically inside the folder, called "goldnormengavg").

To use follow these steps:

* Load Nika and select in SAS2D > Instrument Configurations > APS 12ID-C SAXS with Gold detector
* Instructions will be displayed as well as Open file dialog which is looking for spec file. This is typically file name with two letters followed by two digits for each of day, month, and year (e.g., tl081418) and no extension. This file contains record of exposures and parameters for each image collected. Select this file and it will be imported in Igor in lookup table.
* Next dialog is choice of reading beamline parameters and/or mask definition into Nika - this is available ONLY if file called "goldnormengavg" is found. This file contains distance, pixel size, beam center, mask and other parameters needed for data reduction. If you read these parameters and/or beamline defined mask, any parameters currently in Nika will be replaced with the new ones.
* You MAY get dialog looking for any of your images to be able to create mask. If you get it, select any of the tiff images with your data and the size of this image will be used to create mask.
* In the tab "Em/Dk" select proper blank (empty) image for your data.
* Configure any other data reduction options and output options in Nika.
* You may want to perform better instrument calibration using AgBehenate image (if available) and/or design your own mask.
* Rest of Nika use is same as with other instruments. Note, that Nika will, for each image, pull from records normalization values (I0, I0 for blank), calculate transmission (using Blank image selected) and also pull wavelength. No other parameters are routinely pulled from records. Sorely missing is obviously thickness and any absolute calibration constant. They are not available. You can choose to calculate absolute intensity calibration parameter if you have standard (e.g., Glassy Carbon) measurement available.
* If you need some other parameters from the spec file - like LakeShore temperature, motor positions, etc. - the lookup table is in root\:Packages\:Nika_12IDCLookups in waves with names provided by beamline. You can display the table or write a piece of Igor code which will utilize these values as needed.


.. _Nika.12IDB_Instrument:

.. index::
    Nika; 12ID-B instrument support


12ID-B SAXS WAXS
----------------

This code may or may not work at this time. We are still working some details on how to move data from beamline software to Nika. Some test case provide do work, but some do not.




.. _Nika.ALS_RSoXS:

.. index::
    Nika; ALS RSoXS instrument support


RSoXS ALS soft energy instrument
--------------------------------

This is support for ALS RSoXS instrument. When selected, it allows users to use custom procedures for this instrument. Instructions are provided when user selects "Use RSoXS modifications" checkbox.

.. _Nika.SSRL_MatSAXS:

.. index::
    Nika; SSRL Mat SAXS instrument support


SSRL Mat SAXS
-------------

This is support for SSRL Materials science SAXS camera. When selected, it sets fixed parameters for this instrument and also sets up lookup functions appropriate to read header values recorded in this image format.

.. _Nika.TPA:

.. index::
    Nika; TPA instrument support

TPA
---

This supports data from Australian SANS instrument. Not much more details provided yet and this code is not under development.


.. _Nika.SMI_NSLSII:

.. index::
    Nika; SMI NSLS-II instrument support


Soft Matter Interfaces SMI at NSLS-II
-------------------------------------

This instrument - 12-ID SAXS/GISAXS instrument (https://www.bnl.gov/ps/beamlines/beamline.php?r=12-ID) can generate data which conform to 2D calibrated Nexus canSAS standard. Nika can load these and generate circular or sector profiles or lineouts along arbitrary line. To do this, check "Calibrated 2D data?" and select canSAS/Nexus as image type. Note, that when using input Calibrated 2D data, your data processing is severely limited. Also, at this time the beam center must be in the image or Nika will not be able to get properly azimuthal angles. It probably can be fixed if needed, so let me know if you run into troubles.

If you have other canSAS/Nexus data from another instrument, please, provide me with sample. There seems to be just enough flexibility in the standard, that I cannot guarantee that Nika can read them without testing and possibly tuning the code.


.. _Nika.5ID_DND:

.. index::
    Nika; APS 5ID DND SAXS/WAXS instrument support


DND CAT (APS 5ID) SAXS camera
-----------------------------

DND CAT provides users with data, which are organized in specific folder structure. The data are reduced using scripts based on GSAS-II at the beamline. However, if users wants to process data later in different manner, they have to contact beamline staff and whole process is cumbersome.

Nika DND support is build on presence of evaluated data in text file, where header contains all necessary information for data reduction. Therefore, user opens this text file and the Tiff file with the processed image is found automatically (if user did not change the folder structure). Alternatively, user can point the Nika to the image files, when asked.

The data can then be reprocessed – for example different sectors can be analyzed etc.

Note, that the user needs to make a new mask, but other parameters (beam center, wavelength, calibration constant as well as sample transmission and thickness) are loaded from the header.

The following are instructions which you will get when you select: **"SAS 2D"->"Instrument configurations"--> "DND CAT"**

*Instructions for use of DND CAT special configuration*

0. Open Nika's main panel, if needed.

1. Select "DND/txt" as image type. Check "Display only" as processing method so you do not get errors if mask/parameters are not correct.

2. Using "Select data path" load one txt file located in .../APSCycle/YourName/Month/processing/plot_files, these are the txt files you want to see in the file list. Nika will find tiff files on its own.

        Note, you can load DND processed 1D ASCII data from these files directly into the Irena package using ASCII loader. Q is second column, Intensity is third and error is fourth. Nika is needed only if you want to reprocess the 2D->1D data again, for example if you need sector averages, different mask, etc.
        ´
3. Now, run the Configuration function again... Select in the "SAS 2D"->"Instrument configurations"--> "DND CAT". Select name.txt file with the same name as tiff file you want to process. This will configure the Nika properly (for that detector!!!, there are 3 detectors on DND SAXS), including wavelength, distance, etc. Correct checkboxes will be checked and functions set to provide same data processing as DND suggests to do (see below).

4. Create mask. You need to create it or load it if you have already created it. Make sure you use the correct image file to create it - with the three different image files associated with each sample, it is bit complicated. Nika does not like when mask and image dimension do not match.

5. Set Nika processing & output options you want = set tabs "Sect.", "LineProf" and "Save/Exp". Set Processing options (checkboxes), likely you need "Process sel. files individually"

6. To reduce image, select the text file with the same name as the tiff file you want to process and "Process image(s)". Nika will parse parameters (wavelength, calibration values, thickness,...) from this txt file, locate the tiff file, load it, and process as described. If you do circular average, you should get what the text file contains. It is good to check that you actually get the same output before using Nika to do different types of processing (e.g., sectors). If something does not match, let me know...


This document contains also description I obtained for DND CAT on how data should be processed as well as information where strings with the header from each text file are, in case you need more parameters.
