.. _export_data:

.. index:: Export data

Export data
===========

Irena produces large number of data sets, which can sometimes be exported when created, but more often not. While the most convenient use of the data is within Igor experiment by ploting or processing further, many users may want to use another programs. And then it is imperative to export them as ASCII files. This is the tool for that…

.. image:: media/DataExport1.jpg

   :align: center
   :width: 480px


Above are standard set of controls. Select data in the top part of the panel. Than decide – do you want to see graph of the data? Do you want to see any associated notes (Irena writes a lot of stuff in the wave notes)?

Push button “Load data”

Note the “Export multiple data sets?” checkbox. It opens the Multiple Data Export selection panel. This panel enables exporting of many data sets at once. The correct use of this option is to export one data set manually (sets all parameters and export paths), test one data set and then use the Multiple Data set option…. See comments later.

.. image:: media/DataExport2.jpg
   :align: center
   :width: 480px


Now we have graph and list of notes. Note, that no attempt is made to create sensible graph. You may have to modify the graph manually if needed.

Next select Output options:

**Select what type of data you want to export. Choices are:**
  1.  ASCII data
  2.  GSAS-II compatible (ASCII) xye data
  3.  Nexus (HDF5) data using Nexus NXcanSAS definition

***File type descriptions:***

**ASCII data** are data exported as ASCII (=text) with header information (see below for header separator) in columnar format, columns are separtated by tabs (white space). Exported can be anything - Q/Int/Uncertainty, Size distribution, Model fits,... Anything X-Y-(E) data can be exported this way - and imported in other packages. No conversions are done - what units and data type is selected, that is exported. This is most flexible and compatible export tool.
*NOTE* DO NOT export slit smeared USAXS data - as of 2017-11 there is no package for data analysis I know about which ha s correctly implemented slit smearing compatible with my USAXS data. Export desmeared data or use Irena pac kage for analysis.

**XYE GSAS-II compatible** are ASCII data specially formated so GSAS-II package can load them in. The tool will take Qvector - or - d spacing - or - Two theta + Intensity and Uncertainty data and export them with header in manner which is compatible with "xye" imported in GSAS-II and likely other powder diffraction/WAXS packages. Any input data care converted to TwoTheta-Intensity-Unceertainty and exported with proper header. Note that this is really useful ONLY for powder diffraction (WAXS) data reduced by Nika package, it is not useful for SAXS or USAXS!

**Nexus** exports Nexus NXcanSAS data. This is HDF5 contained with data written in such way, that they are easily exchangeable among different software packages. This is future of SAS data formats. Irena can import such data if needed back - with all metadata properly passing through export and import. Since metadata names and keywords are defined in standard, all including units should be exchanged easily... SHOULD. *NOTE* DO NOT export slit smeared USAXS data - as of 2017-11 there is no package for data analysis I know about which ha s correctly implemented slit smearing compatible with my USAXS data. Export desmeared data or use Irena pac kage for analysis.

**Other controls**

*Attach notes* about data will attach the wave note into the ASCII file. Note, at the bottom of the panel is field where one can insert the separator character (including spaces) if different than default is desired.

*Use folder names for output* - if you are using folder names as anmes for samples, this is sensible…

*Use Y wave name for output* - if your Y wvae name is sample name (e.g., qrs data this type). rarely useful.

*Set export folder* set where to store data. Cannot create folder, create first, then set here. The folder is displayed din red box above the button.

*Export file name* modify, if default is not good enough

*Export file extension* set to .dat for ASCII, .xye for GSAS-II data, and to .h5 for Nexus. For ASCII can be modified as needed. Leave the other as they are.

*Header separator* - useful for ASCII only, change if different isd esired. Include spaces, if these are desired!!!

***"Export Data & Notes"*** button does the job. If the data in the target location exist, you will be asked if you want to overwrite them. It may be easier to delete files from the target location instead of overwriting, if you need to overwrite many. 

Multiple data set export option:

.. image:: media/DataExport3.jpeg
   :align: center
   :width: 280px


There are few items one needs to know about this tool.

1. If you make changes to the main panel, the list of folders in this panel may get stale. Use button “Update list” to update it.

2. There is logic in listing the data which is actually quite complicated… Here are some comments:

a. The tool started to search for data from parent folder of data selected in the main panel. In the current selection :

.. image:: media/DataExport4.jpeg
   :align: center
   :width: 280px


The tool start searching from root:USAXS:USAXS\_WMU: - if you cannot find your data, select different starting folder in the main panel and update the list. This is to reduce clutter and help users with giant experiments…

For **Irena results** The tool will search for not only the same data type as selected in the main panel, but also same generation! Therefore, if you have in some folders saved multiple results from same tool (you have waves with results like: SizesVolumeDistribution\_0, but in some also SizesVolumeDistribution\_1, SizesVolumeDistribution\_2, etc…) the tool will search only for generation (“\_0”, “\_1”,…) selected in the main panel. It just gets really messy to create different logic.
