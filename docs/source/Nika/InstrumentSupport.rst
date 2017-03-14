.. index:: Instrument support Nika

Instrument support
==================

Instrument support are packages of specific additions in Nika to support special instrument. Depending on the instrument, these functions add capabilities and modify settings to make support of specific instrument easy.

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

9ID/15ID SAXS - WAXS
--------------------

This is support for APS beamline 9ID SAXS and WAXS instruments. This is my beamline and there is special handout for how to use this tool.

SSRL Mat SAXS
-------------

This is support for SSRL Materials science SAXS camera. When selected, it sets fixed parameters for this instrument and also sets up lookup functions appropriate to read header values recorded in thsi image format.

TPA
---

This supports data from Australian SANS instrument. Not much more details provided yet and this code is not under development.

For other instrument scientists:

Other instrument setups can be added on request. Provide me with enough data and description and I can write support for your instrument.
