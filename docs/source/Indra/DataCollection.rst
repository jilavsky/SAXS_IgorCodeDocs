.. _indra-data-collection:
.. _introduction_usaxs:

Data Collection
===============

.. index::
    Indra; USAXS data collection
    HDF5 data files
    Nexus files

The USAXS-SAXS-WAXS instrument uses three independent devices: Bonse-Hart
USAXS, pinhole SAXS, and WAXS. Data collection proceeds on one device at a
time, requiring each device to move in and out of the beam position. Two
typical strategies are used:

* **Static samples:** All samples are measured with USAXS first, then the SAXS
  device moves in for all samples, then WAXS. This is the most time-efficient
  sequence.
* **Time-resolved / temperature-resolved experiments:** USAXS, SAXS, and WAXS
  are collected sequentially for each sample before repeating the cycle. The
  overhead for device movements is larger in this case.

Each USAXS and SAXS segment includes a short transmission measurement
performed before each scan. Transmission data are included automatically in
the metadata.

Folder structure
----------------

Data are saved in a specific folder structure inside the base user folder
configured by staff. For example::

    12_5_UserName/
        DataSetName/          (default name: "data")
            DataSetName_usaxs/
                ... many HDF5 files (.h5)
            DataSetName_saxs/
                ... many HDF5 files (.hdf)
            DataSetName_waxs/
                ... many HDF5 files (.hdf)

Users can create as many ``DataSetName`` folders as needed. Each folder can
contain many datasets (even thousands).

Filenames
---------

Sample names are used as HDF5 file names and must meet the following
requirements: start with a letter, contain only letters, numbers, and ``_``,
and be no more than approximately 40 characters long. The system will
automatically remove any unacceptable characters from user input.

For time-, pressure-, or temperature-resolved experiments, a control variable
value is typically appended to the name — for example, ``_246degC_526min``.

An order number guaranteeing uniqueness (e.g., ``_0001``) is appended to each
filename automatically.

Metadata
--------

Each Nexus file contains a large amount of metadata in fields with
human-readable names. You can browse this metadata using free tools such as
HDFView, Igor Pro, or other HDF5 browsers. The *Metadata Browser* tool in
Irena can also quickly extract specific metadata fields.

.. index::
    Indra; USAXS/SAXS/WAXS HDF5 files
    HDF5 files

HDF5 Nexus data files
---------------------

Each data collection segment saves data in a separate HDF5 file. SAXS and WAXS
files follow the NXsas definition
(https://manual.nexusformat.org/classes/applications/NXsas.html). There is no
formal definition for Bonse-Hart USAXS instruments; the USAXS files follow a
loose NXsas convention with modified data formats. The Matilda automatic data
reduction system (see below) appends reduced data following the NXcanSAS
definition
(https://manual.nexusformat.org/classes/applications/NXcanSAS.html#nxcansas)
into each file.

.. index::
    Indra; Matilda automatic data reduction

.. _automatic_data_reduction:

Automatic Data Reduction ("Matilda")
-------------------------------------

Since September 2025, a Python script called *Matilda*
(https://github.com/APS-USAXS/Matilda) has been running to reduce data
automatically for users. Matilda reduces each dataset and appends the results
to the raw data file in NXcanSAS format.

.. note::

   Automatic data reduction may fail due to user errors, code issues, or weak
   data. **Primary data reduction remains the Indra and Nika packages in
   Igor Pro.** When in doubt, verify results using Indra and Nika and compare
   to the automatic output. The results should be close, though minor
   differences may arise from parameter choices.

If non-default reduction parameters are needed, use Igor Pro. Examples of
parameters that can only be changed in Igor: calibration method (Matilda uses
sample thickness for absolute intensity in cm\ :sup:`2`/cm\ :sup:`3`; Igor
supports per-gram calibration using density/weight, or thickness from
transmission); number of Q bins (Matilda defaults to 500 for USAXS and 200 for
SAXS); and other advanced options.

Important — Background data collection strategy
-----------------------------------------------

*These requirements must be met for automatic data reduction to work correctly.*

1. An appropriate background measurement ("Blank") must be available in the
   data folder when sample data are collected.
2. Data reduction proceeds correctly when:

   a. The **most recent Blank** before each sample measurement is used as its
      background.
   b. When a new Blank is collected, it is applied to all subsequent samples.
      Samples measured before the new Blank was collected use the Blank that
      preceded them.
   c. Any measurement with "blank" anywhere in its name (case-insensitive) is
      treated as a Blank — for example: "AirBlank", "Air Blank", "blank",
      "Capillary blank". If "blank" does not appear in the name, the
      measurement is treated as a sample.
   d. If no measurement has "blank" in its name, automatic data reduction is
      not performed.
   e. The Blank must be somewhere in the *user folder*; it does not have to be
      in the same sample subfolder.
   f. Samples without a matching Blank are reduced only to QR data (no
      background subtraction, no calibration) and must be re-reduced in Igor.

3. Sample calibration uses the **thickness** value stored in the file. There
   is no way to patch this after data collection — thickness must be provided
   before collecting data.
4. Calibration assumes thickness is the correct method, giving intensity in
   units of cm\ :sup:`2`/cm\ :sup:`3`. Calibration per weight is not
   available in Matilda.
5. Data reduction runs automatically within 15 seconds after each dataset
   finishes collecting.

*Example of a proper data collection sequence:*

::

    Sample set 1
        1. newSample("Set1")
            1. measure Blank1
            2. measure samples belonging to Blank1  (*)
            3. measure Blank2
            4. measure samples belonging to Blank2  (*)
    Sample set 2
        1. (Optional) RE(newSample("Set2"))
            1. Recommended: measure Blank3
            2. measure samples belonging to the last Blank measured  (*)
    Sample set 3
        ...

(*) Collect a new blank every 10–20 samples or every hour for standard
resolution (Si 220), or every 5–10 samples or every 30 minutes for high
resolution (Si 440). If this is not possible, collect a new blank as soon as
convenient.

What data reduction Matilda does
---------------------------------

1. Detects a new USAXS/SAXS/WAXS data file on the server after measurement
   completes.
2. Reads the file name and order number (``_XYZ`` suffix before the extension).
3. Identifies the most recent Blank in the same *userName* folder based on the
   ``_XYZ`` number. The blank with the closest lower order number is selected.
4. Follows the same reduction path as Igor:

   a. Reduces both Blank and Sample to Q–Intensity–uncertainty data.
   b. Calculates transmission and calibration factors:

      * For USAXS: standard-less calculation from first principles.
      * For SAXS and WAXS: uses a Glassy Carbon SRM 3600 measurement to
        determine the calibration constant.

   c. Subtracts the Blank from the Sample and applies calibration constants,
      producing USAXS slit-smeared (``SMR``) and SAXS/WAXS (``QRS``) data.
   d. Rebins data: USAXS flyscans (~8k points) to 500 log-Q bins; SAXS (~800
      points) to 200 log-Q bins; WAXS is kept at maximum resolution.
   e. For USAXS, desmears the data (``DSM``) to produce pinhole-equivalent
      data suitable for any analysis tool.

5. Appends the reduced data to the raw Nexus file in this order:

   a. Sample QRS data
   b. Blank QRS data
   c. Calibrated data (NXcanSAS format)
   d. For USAXS: slit-smeared data in NXcanSAS slit-smeared format.

Data files — next steps
------------------------

After data collection you have HDF5 files containing both raw (NXsas) and
reduced/calibrated (NXcanSAS) data. If Matilda ran correctly and no custom
parameters are needed, these files are all you need.

If anything needs to be changed or re-reduced, use Igor Pro (Indra for USAXS,
Nika for SAXS and WAXS). Igor allows overriding many default parameters,
including selecting a different Blank (even measurements without "blank" in
their name).

Options for using these HDF5 data:

1. Import into Igor using the new USAXS GUI, the HDF5 importer, or native
   Igor Pro HDF5 handling. See :ref:`Import data <import_data_procedure>`.
2. Use the HDF5 files directly in applications that read NXcanSAS — for
   example, SasView 6 (https://www.sasview.org/) can open these files and
   automatically locate the relevant data.
3. Open the files in HDFView to inspect the data structure and metadata. Any
   application that reads HDF5 (e.g., MATLAB) can import the data.
4. Use Python to read the data for analysis. The NXcanSAS definition
   describes how to locate data using attributes. The Matilda repository
   (GitHub) contains example code for reading these files.
