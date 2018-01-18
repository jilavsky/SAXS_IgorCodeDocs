.. _merge_data_procedure:
.. _merge_data_panel:


.. index::
    Merge USAXS/SAXS/WAXS data
.. index::
    USAXS/SAXS/WAXS data merge

Merge USAXS/SAXS/WAXS data
==========================

When you collect data on USAXS instrument, your data are saved in folders related to your "spec" file name. Spec file is where instrument makes various records. The file name is created by adding MM_DD_ (month_day_) to the name staff provides, typically to user name. When you collect USAXS data, a folder with the same name with appended "_usaxs" is created. For SAXS data we create folder with the same name with "_saxs" and for  WAXS with "_waxs". See below in the figure:

.. image:: media/USAXSComputerDataArrangement.jpg
        :align: center
        :width: 480px

After you reduce data from USAXS instrument, you will have in your Igor experiment data arranged in data folders also - in this case you will have USAXS data in root\:USAXS\:Samplename.
To see inside of the current Igor experiment, use DataBrowser (ctrl-B or cmd-B).

.. image:: media/USAXSComputerDataArrangement.jpg
        :align: center
        :width: 480px


.. index::
    USAXS data panel

Data reduction panel
--------------------

Select "Load USAXS macros" from "Macros" menu. This will create "USAXS" menu and also open "Read me" notebook. Note, that it will take some time to compile the code, depending on the speed of your computer. Select "Import and reduced USAXS data" from the "USAXS menu".

.. Figure:: media/DataReduction1.jpg
        :align: left
        :width: 800px
        :Figwidth: 820px
