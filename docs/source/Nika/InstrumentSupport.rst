.. _Nika.InstrumentSupport:

.. index:: Instrument support (Nika)

Instrument support
==================

Instrument support are packages of specific additions in Nika to support special instrument. Depending on the instrument, these functions add capabilities and modify settings to make support of specific instrument easy.

Currently there are these instruments supported:

1.  :ref:`APS 9ID-C USAXS/SAXS/WAXS <Nika.9IDC_Instrument>`
2.  :ref:`APS 12ID-C SAXS/WAXS <Nika.12IDC_Instrument>`
3.  :ref:`ALS RSoXS soft energy SAXS <Nika.ALS_RSoXS>`
4.  :ref:`SSRL Mat SAXS <Nika.SSRL_MatSAXS>`
5.  :ref:`TPA <Nika.TPA>`
6.  :ref:`APS 6ID DND SAXS/WAXS <Nika.5ID_DND>`

.. _Nika.9IDC_Instrument:

.. index::
    Nika; 9ID-C instrument support

9ID/15ID SAXS - WAXS
--------------------

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

For other instrument scientists:

Other instrument setups can be added on request. Provide me with enough data and description and I can write support for your instrument.

.. _Nika.5ID_DND:

.. index::
    Nika; APS 5ID DND SAXS/WAXS instrument support


DND CAT (APS 5ID) SAXS camera
-----------------------------

DND CAT provides users with data, which are organized in specific folder structure. The data are reduced using scripts based on fit2d at the beamline. However, if users wants to process data later in different manner, they have to contact beamline staff and whole process is cumbersome.

Nika DND support is build on presence of evaluated data in text file, where header contains all necessary information for data reduction. Therefore, user opens this text file and the Tiff file with the processed image is found automatically (if user did not change the folder structure). Alternatively, user can point the Nika to the image files, when asked.

The data can then be reprocessed – for example different sectors can be analyzed etc.

Note, that the user needs to make a new mask, but other parameters (beam center, wavelength, calibration constant s well as sample transmission and thickness) are loaded from the header.

To use:

1. select DND/txt as file type and point find data in the right folder. It is likely something like:…/APSCycle/YourName/Month/processing/data/plot\_files

2. select and display one or more of the text files which contains current configuration and display. If the folder structure is correct, tiff image is found automatically. If not, Nika will ask for the image location. It should be necessary only once, unless the images are in different places.

3. Select “Instrument configurations” >> “DND CAT”. Select configuration from the listed names of the text fie(s) which were loaded already. This will set wavelength, pixel sizes, distance, and centers… Also it will set proper calibration configuration and functions, which should be used. Note: Dark was already subtracted from these files, so you need only the few parameters listed (thickness, calibration constant and transmission).

4. Create and use mask.

5. If you have empty run measurements, select the checkbox for “\ **subtract empty”**. Do not change values for “\ **Use I0/I0emp**\ ” or value of 1 for I0. This is important to scale properly Empty and Sample incoming intensities and measurement times.

6. Select proper reduction parameters (circular, sector etc…).
