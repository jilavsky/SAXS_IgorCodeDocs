.. _introduction_usaxs:

Data Collection
---------------

.. index::
    USAXS data collection
    HDF5 Data files
    Nexus Files



USAXS-SAXS-WAXS instrument uses three independent devices, Bonse-Hart USAXS, pinhole SAXS, and WAXS. Data collection can be done only on one device at time, therefore these devices need to switch positions in order to collect different data. There are typically two strategies - for static samples USAXS data are collected on all samples, then SAXS device is moved in and all SAXS data are collected, and finally WAXS moves in and all WAXS data are collected. This is most efficient method for time utilization. For time resolved, temoperature resolved, etc, data we usually collect USAXS, mthen SAXS, and then WAXS data and repeat as necessary. The overhead for movements is larger in this case. 

Note, that USAXS and SAXS data collection has separate short transmission measurement done before each scan. These transmission data are automatically included in metadata. 

Folder structure
================

As data are collected, they are saved in specific folder structure inside base user folder staff sets up for user, for example:

\\ "12_5_UserName" 
    \\ "DataSetName" (default is "data")
        \\ "DataSetName_usaxs"
            \ ... many HDF5 files (extension ".h5")

        \\ "DataSetName_saxs"
            \ ... many HDF5 files (extension ".hdf")
       
        \\ "DataSetName_waxs"
            \ ... many HDF5 files (extension ".hdf")

User can create as many "DataSetName" folders as needed, typically between one and few. Each "DataSetName" folder can contains many (even thousands) of data sets. 

Filenames
=========

Users need to provide suitable names for samples - these are used as HDF5 file names. Therefore they have to start with a letter, cannot contain any special characterm except "_" and for practical purposes need to be relatively short. Reasonable number may be 10-15 characters. 

In case of time, pressure, or temperature resolved experiments we try to append control variable value into the name, in heater experiments we typically append ssomething similatr to "_246degC_526min"

For each data we also append "order number" which guarrantees uniqueness, something like "_0001".

Our code will take user provided name, any additional information, and the order number, removes any unaaceptable characters and makes this into file name. 

Metadata
========

Large amount of metadata is added into each Nexus file in various fields which usually have human understandable names. Users can use free tools, like HDFView or other browsers, like Igor Pro, and find the necessary metadata. There is also "Metadata broser" tool in Irena which can quickly extract necessary metadata. 


.. index::
    USAXS/SAXS/WAXS HDF5 
    HDF5 files


HDF5 Nexus data files
=====================

Each data collection segment saves its data in separate HDF5 file, SAXS and WAXS follow NXsas definition (https://manual.nexusformat.org/classes/applications/NXsas.html). There is no definition for Bonse-Hart USAS instruments, therefore we follow loosely NXsas with different data formats. Matilda data reduction (see below) adds reduced data following NXcanSAS definition (https://manual.nexusformat.org/classes/applications/NXcanSAS.html#nxcansas) in each file. 



.. index::
    Matilda

.. _automatic_data_reduction :   

Automatic Data Reduction ("Matilda")
====================================

From 2025-09 we have been running Python code "Matilda" (https://github.com/APS-USAXS/Matilda) which reduces the for users. Matilda reduces data (see below) and appends the data into files using abovementioned NXcanSAS format. 

This automatic data reduction may fail due to user errors, code errors, or for marginally useful/weak data. *Primary data reduction is still Indra and Nika packages in Igor.* When in doubt, verify using Indra and Nika data reduction packages and only use the automatically generated data if the automatic data match the Igor results. Match should be reasonably close but, perhaps, not perfect, as choices in some parameters may vary.

*NOTE:* If you need different than default parameters in data reduction, use Igor code. Examples are for example: calibration method - Matilda calibrated using thickness for absolute intensity in [cm2/cm3], if you need to calibrate per gram using sample density/weight or if you need to calculate thickness from transmission, this can be done only in Igor. If you need different number of points (default is 500 for USAXS and 200 for SAXS), you can do it in Igor. and there are other parameters one may need to change, all can be done in Igor ONLY.    



Important - Background data collection strategy
===============================================

*These are the requirements of the automatic data reduction routine to work properly*

1. An appropriate background measurement "Blank" must be available in the data folder when sample data are collected. 
2. Data reduction will be correct IF:
    1. The LATEST Blank (last measured) is the correct one for following samples. **THE LASTEST ONE before the sample is measured**.
    2. When a new Blank is collected, it is used for subsequent samples. Prior measured samples will use the Blank measured last before their measurement.
    3. Any measurement with "blank" in name is considered Blank. Case independent and independent of any other qualifiers. "AirBlank", "Air Blank", "blank", "Capillary blank",… all these are BLANK for following measurements. If this string is not there, it is sample measurement.
    4. If no data have "blank" in name, automatic data reduction is NOT done.
    5. The Blank MUST be somewhere *User folder*, it does not have to be in the same SampleFolder. 
    6. Data without a blank will only be reduced to QR data (no background subtraction, no calibration) and need to be reduced in Igor.
3. Sample calibration is done using the sample **thickness** in the file. There is no way to patch parameters back, thickness must be prvided before data collection.
4. Calibration is done assuming the *thickness* is the correct calibration method. That means the Intensity units are [cm2/cm3]. There is no way to do calibration per weight etc.
5. The data reduction/calibration is automatically done within 15 seconds after the data are finished collecting.

*Example of a proper data collection sequence:*

Sample set 1
    1. newSample("Set1")
        1. measure Blank1
        2. measure Samples belonging to Blank1, see (*)  
        3. measure Blank2
        4. measure samples belonging to Blank2, see (*)
Sample set 2
    1. (Optional) run: RE(newSample("Set2"))
        1. Good pracice: measure Blank3
        2. measure samples belonging to the last Blank measured, see (*)
Sample set 3
    ...


(*) It is strongly suggested to collect a new blank every 10-20 samples or every hour or so for Standard resolution (Si220) and 5-10 samples or every 30 minutes for High resolution (Si440). If this is not possible a new blank should be collected as soon as convenient.
 

What data reduction Matilda does
================================

1. Gets a new USAXS/SAXS/WAXS data file info from server AFTER it is finished measuring
2. Reads the file name and order number (_XYZ at the end of name before extension).
3. Identifies the last Blank in the same *userName* folder based on "_XYZ" number. The *blank with the closest lower number is selected!*
4. Follows same "path" as Igor data reductions:
    1. Reduces both Blank and Sample to Q-Intensity-uncertainty data.
    2. Calculates transmission and calibration factors. 
        a. For USAXS this is standard-less calculation based on first principles.
        b. For SAXS and WAXS we use Glassy Carbon SRM3600 measurement to calculate calibration constant.  
    3. Subtracts the blank from the sample and applies calibration constants (creates USAXS slit smeared ("SMR") and SAXS/WAXS "QRS" data)
    4. For USAXS Matilda rebins the collected data (8k points for Flyscans) into 500 log-q spaced bins. For SAXS we rebin data (about 800 points) into 200 log-q spaced bins. For WAXS we keep as many points as possible.   
    5. For USAXS Matilda desmears the data ("DSM") creating pinhole-equivalent data suitabel for any data analysis tool. 
5. Appends the data to the raw data Nexus file in the following order:
    1. Sample QRS data
    2. Blank QRS data
    3. Calibrated data (following NXcanSAS standard)
    4. For USAXS also appends NXcanSAS Slit smeared data version following NXcanSAS definition for slit smeared data.


Data files - what to do now? 
============================

OK, so now you have bunch of HDF5 files, what to do with them? These are combined raw (NXsas) and reduced/calibrated (NXcanSAS) data at once, not likely common thing for most users... 

Believe or not, these are all data you really need, if you have done everything right and do not have any special requirements. Keep in mind that Matilda has default parameter selection (see above) which cannot be changed. If anythign needs to be changed or modified, Igor code (Indra for USAXS nadn Nika for SAXs and WAXS) are tools which can be used to re-reduce the data. In Igor one can overwrite many selections and even change the Blank into any other measurement (including measurements which do not have "blank" in name).

**Here are options how to use these HDF5 data:**

1. Import into Igor through the new USAXS GUI, HDF5 importer, or native Igor Pro HDF5 handling. See :ref:`Import data <import_data_procedure>`
2. Use the hdf5 files directly in applications, which can read NXcanSAS data - I have tested sasView and it worked perfectly fine. Throw the HDF5 file into file window and sasView picked the right data from it.  
3. Open the file in HDFView and look at the data, check the file structure, find metadata. Use any application able to read HDF5 files to import the data (Matlab etc).  
4. Use Python to read the data from the file for analysis in Python. Note, that NXcanSAS definition describes how to find where the data are the from attributes, if needed, there is code in Matilda (check Github page) which does this for you.

