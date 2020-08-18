.. _dataDescription:

.. index::
    USAXS/SAXS/WAXS data description
    Data naming systems

Data collected
==============

This is description of what data are collected by USAXS - SAXS - WAXS instrument and how they are stored in Igor experiment.

There are two basic types of measured data - "USAXS" and "QRS" which you need to distinguish when looking inside Igor Experiment with reduced data from this instrument. Irena also recognizes "Irena results" as type of Irena output tools - Size distribution, Unified fit, etc.

.. index::
    USAXS data type

USAXS data type
---------------

Independent if you used Fly scanning or step scanning, your data will be of type "USAXS". Data will be found in root\:USAXS folder, organized in folder, one folder for each data collection run - example: *SampleName_0001* . Inside this SampleName_0001 folder you will find many waves with data, but there are two which are of real importance :

    1. SMR_Qvec, SMR_Int, SMR_Error - these are Q, Intensity, and uncertainty for your data, in SLIT SMEARED geometry. Slit length (typically 0.025 - 0.35 1/A) is in the wave note. These data should be on absolute intensity scale, if you had proper sample thickness provided to code during data reduction. *Irena* can use these data for modeling, as it has slit smearing built in the code, and will switch it on when necessary.
    2. DSM_Qvec, DSM_Int, DSM_Error - these are Q, Intensity, and uncertainty for your data in pinhole geometry. These are obtained either by desmearing (during data reduction or after data reduction using separate tool) - or optionally by using 2D collimated USAXS instrument geometry.




.. index::
    SAXS data type
    WAXS data type

SAXS/WAXS data type
-------------------

SAXS and WAXS use Nika for data reduction and that uses different naming system called QRS. Data will be organized in folder root\:SAXS and root\:WAXS folder, each data collection will have its name there. Ideally the names of associated measurements will be the same, so you should fined inside each a folder with name *SampleName_0001_xyz*. The xyz is code Nika uses to identify type of data reduction used to obtain the data. Here are the codes used:

    1. SAXS data have "270_30". This is Nika's way of saying, this is sector average in 270degree direction (vertically down), +/- 30 degrees width. These are pinhole collimated data for SAXS and should be used directly as pinhole SAXS data or merged with DSM type USAXS data. These data are generated always in our data reduction code.
    2. SAXS data have "u". This is Nika generated "fake" slit smeared data with smearing using same slit length as USAXS data. These data should be used to merge with USAXS SMR type data. There is really no other use for these data. These data are generated only if USAXS data are not desmeared (there is checkbox on the control panel for this).
    3. WAXS data have "C". This is Nika's way of telling it is doing circular average around the beam center, where data exist.

In this :ref:`QRS naming system <QRSdataDescription>`the folder is called : "SampleName_0001_xyz" with "q_SampleName_0001_xyz" for Q vector, "r_SampleName_0001_xyz" for intensity, and "s_SampleName_0001_xyz" for uncertainty.


.. index::
    Merged data types

Merged data types
-----------------

Data merging is done using :ref:`Data Merge tool <data_merge>`. After using this tool, you will have new folders with merged data. These folders are found inside parent folder of the first (left hand) data set and will have type of the fist (left hand) data set. This first (left hand) data set should also be the low-q segment.

Therefore, if you merge:
  * USAXS + SAXS    --- you will have in root:USAXS a new folder called "SampleName_0001_mrg" with USAXS data, either SMR type or DSM type.
  * USAXS + SAXS + WAXS   --- you will have in root:USAXS a new folder called "SampleName_0001_mrg_mrg" with USAXS data, either SMR type or DSM type.
  * SAXS + WAXS   --- you will have in root:SAXS a new folder called "SampleName_0001_270_30_mrg" with SAXS/WAXS data, QRS type.


Therefore :  if you have folder named : *SampleName_0001_mrg*, these are USAXS data merged with SAXS data. They will contain same data types as described above, but extended by SAXS data. These are still "USAXS" data type for Irena and can be used by Irena for modeling. Ideally, this is what you want for USAXS+SAXS. They can be either slit smeared (SMR) or desmeared (DSM), irena handles both transparently.

If you have folder named : *SampleName_0001_mrg_mrg*, these are USAXS data merged with SAXS and WAXS data. They are pretty cool (5 decades in Q) but also useless as you need to analyze them with two different sets of tools.

If you decide to merge SAXS + WAXS data, make sure SAXS is switched to "max number of points" before SAXS data reduction and then merge SAXS+WAXS data. In this case you should have inside root\:SAXS folder a new folder with name *SampleName_0001_270_30_mrg*. These are QRS data for Irena.


Export for other tools
----------------------

*DO NOT export for other tools SRM (slit smeared) data, unless you really know what you are doing.* I was unable to verify, that other tools handle slit smearing we have properly. If you want to do so, talk to me and we will test it first.
Use :ref:`Export data tool <export_data>` and export ASCII or Nexus for SAS and GSAS xye for WAXS tools, like GSAS-II.
