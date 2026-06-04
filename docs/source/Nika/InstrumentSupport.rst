.. _nika-instrument-support:
.. _Nika.InstrumentSupport:

.. index:: Nika; Instrument support

Instrument support
==================

Instrument support packages extend Nika with instrument-specific capabilities
and default settings, making it straightforward to reduce data from supported
instruments.

Currently supported instruments:

1.  :ref:`APS 9ID-C USAXS/SAXS/WAXS <Nika.9IDC_Instrument>`
2.  :ref:`APS 12ID-C SAXS/WAXS <Nika.12IDC_Instrument>`
3.  :ref:`APS 12ID-B SAXS/WAXS <Nika.12IDB_Instrument>`
4.  :ref:`ALS RSoXS soft energy SAXS <Nika.ALS_RSoXS>`
5.  :ref:`SSRL Mat SAXS <Nika.SSRL_MatSAXS>`
6.  :ref:`TPA <Nika.TPA>`
7.  :ref:`APS 5ID DND SAXS/WAXS <Nika.5ID_DND>`
8.  :ref:`SMI NSLS-II <Nika.SMI_NSLSII>`

Support for additional instruments can be added on request. Contact the
developer (ilavsky@aps.anl.gov) with sufficient data and a description of the
instrument geometry.

.. _Nika.9IDC_Instrument:

.. index::
    Nika instrument support; 9ID-C instrument support

9ID SAXS-WAXS
-------------

Support for the APS beamline 9ID SAXS and WAXS instruments. Detailed
instructions are displayed when this configuration is selected from the menu.
A YouTube tutorial walkthrough is also available.

.. _Nika.12IDC_Instrument:

.. index::
    Nika instrument support; 12ID-C instrument support

12ID-C SAXS using Gold detector
---------------------------------

Support for the APS beamline 12ID-C SAXS instrument. Brief instructions are
displayed when the option is selected.

Requirements: a folder of TIFF files, a spec file, and optionally a beamline
data reduction script (typically named ``goldnormengavg``, located inside the
data folder).

Procedure:

* Load Nika and select: SAS2D → Instrument Configurations → APS 12ID-C SAXS
  with Gold detector.
* A file dialog opens prompting for the spec file — typically a filename with
  two letters followed by two digits each for day, month, and year (e.g.,
  ``tl081418``), with no extension. This file records exposures and parameters
  for each image. Select it to import a lookup table into Igor.
* A second dialog asks whether to read beamline parameters and/or a mask
  definition from ``goldnormengavg`` (if present). Importing these replaces
  any existing Nika parameters with the beamline-defined values.
* A third dialog may appear asking you to select any TIFF image from your
  data, so that its dimensions can be used to configure the mask.
* In the "*Em/Dk*" tab, select the appropriate blank (empty) image.
* Configure data reduction and output options as needed.
* Optionally, perform instrument calibration using an AgBehenate image (if
  available) and create a custom mask.
* Nika reads normalization values (I₀, I₀ for blank), calculates transmission,
  and pulls wavelength from the lookup table for each image. Sample thickness
  and absolute calibration constants are not available in the spec file and
  must be entered manually. If a standard (e.g., Glassy Carbon) measurement is
  available, absolute calibration can be performed.
* Additional metadata from the spec file (LakeShore temperature, motor
  positions, etc.) are available in the lookup table at
  ``root:Packages:Nika_12IDCLookups``.

.. _Nika.12IDB_Instrument:

.. index::
    Nika instrument support; 12ID-B instrument support

12ID-B SAXS WAXS
-----------------

Support for the APS beamline 12ID-B SAXS and WAXS instruments. This
implementation is still under development and may not work for all datasets.

.. _Nika.ALS_RSoXS:

.. index::
    Nika instrument support; ALS RSoXS instrument support

RSoXS ALS soft energy instrument
----------------------------------

Support for the ALS RSoXS instrument. When selected, custom procedures for
this instrument become available. Instructions are displayed when the
"*Use RSoXS modifications*" checkbox is checked.

.. _Nika.SSRL_MatSAXS:

.. index::
    Nika instrument support; SSRL Mat SAXS instrument support

SSRL Mat SAXS
--------------

Support for the SSRL Materials Science SAXS camera. Selecting this option sets
fixed instrument parameters and configures lookup functions to read header
values recorded in the SSRL image format.

.. _Nika.TPA:

.. index::
    Nika instrument support; TPA instrument support

TPA
----

Support for data from the Australian SANS instrument. This support is not
under active development; limited documentation is available.

.. _Nika.SMI_NSLSII:

.. index::
    Nika instrument support; SMI NSLS-II instrument support

Soft Matter Interfaces (SMI) at NSLS-II
-----------------------------------------

The 12-ID SAXS/GISAXS instrument at NSLS-II
(https://www.bnl.gov/ps/beamlines/beamline.php?r=12-ID) can produce data
conforming to the 2D calibrated NXcanSAS/NeXus standard. Nika can load these
files and generate circular or sector profiles, or line profiles along an
arbitrary direction.

To use this support: check "*Calibrated 2D data?*" and select canSAS/Nexus as
the image type.

.. note::

   When using calibrated 2D input, data processing options are limited. The
   beam center must also be within the image frame; otherwise azimuthal angles
   cannot be determined correctly. If you encounter issues with other NXcanSAS
   data from a different instrument, please provide a sample dataset — the
   standard has enough flexibility that compatibility cannot be guaranteed
   without testing.

.. _Nika.5ID_DND:

.. index::
    Nika instrument support; APS 5ID DND SAXS/WAXS instrument support

DND CAT (APS 5ID) SAXS camera
-------------------------------

DND CAT provides data in a specific folder structure. Data are typically
reduced at the beamline using GSAS-II-based scripts. If you need to reprocess
data afterwards (e.g., for sector averages or a different mask), the Nika DND
support reads the evaluated 1D text files whose headers contain all parameters
needed for 2D reduction. The corresponding TIFF image is located automatically
if the original folder structure is intact; otherwise you will be prompted to
locate it manually.

Sample thickness, transmission, wavelength, beam center, calibration constants,
and other parameters are all read from the text file header. You must create a
new mask.

Instructions displayed when you select **SAS 2D → Instrument configurations →
DND CAT**:

0. Open Nika's main panel if it is not already open.

1. Select "*DND/txt*" as the image type. Check "*Display only*" as the
   processing method to avoid errors while mask and parameters are still being
   configured.

2. Use "*Select data path*" to load one ``.txt`` file located in
   ``.../APSCycle/YourName/Month/processing/plot_files``. Nika will locate the
   corresponding TIFF files automatically.

   .. note::

      The 1D ASCII data in these text files can be loaded directly into Irena
      using the ASCII loader (Q = column 2, Intensity = column 3, Error =
      column 4). Nika is only needed if you want to reprocess the 2D → 1D
      reduction, for example to apply a different mask or compute sector
      averages.

3. Run the configuration function again: SAS 2D → Instrument configurations →
   DND CAT. Select the ``.txt`` file with the same name as the TIFF you want
   to process. This configures Nika for the correct detector (there are three
   detectors on the DND SAXS) and sets wavelength, distance, and all other
   relevant parameters.

4. Create or load a mask. Ensure the mask dimensions match the image
   dimensions — Nika will not process images if the mask and image sizes differ.

5. Set processing and output options in the "*Sect.*", "*LineProf*", and
   "*Save/Exp*" tabs. Enable "*Process sel. files individually*" as needed.

6. Select the ``.txt`` file matching the TIFF to process and click
   "*Process image(s)*". Nika reads all parameters from the text file, locates
   the TIFF, and processes it. Verify that the output matches the text file
   contents before using Nika for non-default processing.
