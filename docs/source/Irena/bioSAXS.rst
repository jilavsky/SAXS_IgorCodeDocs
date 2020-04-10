.. _bioSAXS:

Bio SAXS support
================

List of Tools
----------------

1.  :ref:`Import bioSAXS ASCII data <import_bioSAXS_ASCII>`
2.  :ref:`Average, Subtract, Scale  <average_subtract_scale>`
3.  :ref:`Basic Fits <data_merge>`

.. _import_bioSAXS_ASCII:

.. index:: Import bioSAXS ASCII data

Import bioSAXS ASCII Data
-------------------------


This chapter describes how to import bio SAXS 1D SAS in Irena. Irena has other importers - for ASCII, HDF5 canSAS Nexus, & canSAS XML - suitable for most non-bio type data. These are described here :ref:`Import Data <import_data>`

*Users of Indra 2 or Nika produced data can skip this chapter.*


**Function description:**

This ASCII 1D data loader is custom made for specific ASCII data type. Please check that your data are compatible with these requirements.

- ASCII with two or three columns of data. Optional text block separated by separator (many different supported) will be skipped and ignored.
- First column Q vector in [1/Angstrom], optionally in [1/nm] which can be converted into [1/Angstrom] which Irena uses.
- Second column Intensity (ideally absolute, but relative is fine)
- Third column (optional) is uncertainty (error) for intensity. If there is no uncertainty, it will be created as 4% of intensity value.
- Typically (optional) more files per one sample, files are designated by "_00001" number at the end of the name (number length does not matter, separator _ is required). All characters before this last number are considered sample name.
- Sample names are unique and not excessively long. Suggested limit is 20-25 characters or less. This is GUI limit, not Igor 8 limit.
- Default extension expected is dat, but can be changed in the GUI.


**Example of valid names**

::

  Buffer_01 measurements:
      Buffer_01_00001.dat, Buffer_01_00002.dat, Buffer_01_00003.dat, etc...
  Sample1_035 measurements:
      Sample1_035_000001.dat, Sample1_035_000002.dat, Sample1_035_000003.dat, etc.

**Example of valid data file content**

::

 % Filename : Sbuf1_00033_00003.tif
 % Date & Time : 19-Feb-2020 15:24:09
 % X-ray Energy (keV) : 13.300
 % Exposure Time (s) : 0.500
 % Beam Center : 741.50700, 216.72300
 % Sample to Detector Distance (SDD) (mm) : 2007.410
 % Detector Pixel Size (mm) : 0.172
 % Photodiode Value : 15670.500
 % I0 of Sample : 1.244850e+04
 % I0 of Standard : 1
 %
 % q(A^-1)   I(q)    sqrt(I(q))
 3.50000000e-03	0.00000000e+00	0.00000000e+00
 3.62054899e-03	1.76072986e+00	1.28270598e+00
 3.74524999e-03	1.27342930e+00	6.58838643e-01
 3.87424601e-03	1.59209666e+00	1.10772011e+00
 etc.


Importing ASCII SAS data
------------------------


.. image:: media/ImportDataBio1.jpg
        :align: center
        :width: 280px

Select ASCII data import from “BioSAS” menu. You get GUI, which presents various options described below.



.. Figure:: media/ImportDataBio2.jpg
        :align: left
        :width: 300px
        :Figwidth: 320px

Explanation of control available here:

“\ *Select data path”* browse to the folder on the computer drive where the data for import are located.

“\ *Data path”* this shows the path selected above. Cannot be edited in this window, use button *Select data path* to change the path if needed.

"\ *Match name"* enables to use string to show in the listbox only subset of files.

“\ *List of available files”* lists all files in the current folder on the computer, unless masked by *Data extension*. One or more files here can be selected for import. Use shift - click to select multiple files (on Windows) or cmd – click on Macs (to pick one file at time), shift-click to pick range of files. Double click on file runs "Test" and "Preview" commands on that file.

“\ *Data extension”* if extension is put in this filed (e.g., “dat”) only files with the “dat” extension will be shown in the *List of available files*.

“\ *Preview”* Test import of first selected file. Not really necessary, but very useful. Will display graph, if it looks OK, you should have no problems reading the files.

”\ *Select all”* or “\ *Deselect all”* modifies which files are selected in “\ *List of available files”*.

”\ *SAXS data?”* or *WAXS data?* select if you areimporting SAXS or WAXS data. All this does is it places data folders in either root\:SAXS or root\:WAXS folders for easy orientation. It also enables you to have same file names for SAXS and WAXS data. NOTE: You can merge SAXS and WAXS using Irena Megre data tool.

\ *“Convert Q from [1/nm]”* select if units used in file for Q are [1/nm]. Units will be converted to A\ :sup:`-1` if nm\ :sup:`-1` data are imported. Irena uses A\ :sup:`-1`.

“\ *Note on errors”* if the data imported do not contain error bars, this tool will generate 4% Intensity errors.

NOTE: If the data contain header of data (typically number of lines with special character, such as #, $, ... at the start of the line and some spaces before useful information, this ASCII importer will simply ignore them.

**Use of the ASCII Import tool:**

Locate data using “\ *Select data path”* button. This will populate the listbox on the left hand side. Double click any file to generate preview graph (or select file and push button “\ *Preview”* which will do the same thing). If the graph looks OK - check the Q units at this moment - the tool will import the data without issues. If there are weird things and something does not look right, you can try using Irena ASCII importer in menu SAS>Data Import Export>Import ASCII SAS Data. It has lot more functionality and you can probably import the data that way. read the manual on this tool...

.. Figure:: media/ImportDataBio3.jpg
        :align: left
        :width: 300px
        :Figwidth: 320px

So, lets assume the graph looks OK. Select files which you want to import - or just select all using button "Select all".

Next decide, if you have many files per one sample - typically multiple measurements you want to average first - or if you have one file per sample. If you have many files (our example) you should check "Group by Samples?" option. If you have one file per sample, you should uncheck this checkbox or your data structure will be too complicated.

If the "Group by Sample?" is checked, code will assume that string before the last number separated by "_" - that is before "_00023.dat" is the name and create subfolder for that sample. That is **VERY convenient** in this case, you'll see it later. See in the figure below, how the data structure looks like: your data were imported in root\:SAXS. In there, for each sample name code created folder with name based on the file name (without the last "_000xx" number). It placed all individual data inside its own folders with names which now2 include that last number to make sure the names match the file names. Inside each individual folder code placed your q values in wave called "q_sampleName", intensity in "r_samplename" and errors in "s_samplename". This is what is knowns as QRS naming system Irena uses :ref:`QRS naming system <important.QRS>`.

However, if you have only one measurement per sample, using this grouping just buries your data to deeper folder structure. In that case, do NOT do it, it will just keep annoying you.


.. Figure:: media/ImportDataBio4.jpg
        :align: left
        :width: 300px
        :Figwidth: 320px




.. _average_subtract_scale:

.. index:: bioSAXS Average, Subtract, Scale

Average, Subtract, Scale bioSAXS Data
-------------------------------------


This chapter describes how to use Average, Subtract, Scale tool for bioSAXS data. Irena has other Data manipulation tools. These are described here :ref:`Data Manipulation 1 <data_manipulation_1>` and :ref:`Data Manipulation 2 <data_manipulation_2>`
