.. _reduce_SAXS_data_procedure:
.. _reduce_SAXS_data_panel:


.. index::
    Reduce SAXS data
.. index::
    SAXS data reduction

Reduce SAXS data
----------------

*Please note, that first step of data SAXS data reduction should be to reduce USAXS data. The presence of USAXS data provides needed information, such as slit length of the instrument. Your life will be much easier if you reduce USAXS data first.*

Collected data arrangement
==========================

When you collect data on 9IDC USAXS/SAXS/WAXS instrument, your data are saved in folders related to your "spec" file name. Spec file is where instrument makes various records. The file name is created by adding MM_DD_ (month_day_) to the name staff provides, typically to user name. When you collect USAXS data, a folder with the same name with appended "_usaxs" is created. For SAXS data we create folder with the same name with "_saxs" and for  WAXS with "_waxs". See below in the figure:

.. image:: media/USAXSComputerDataArrangement.jpg
        :align: center
        :width: 480px

Reduced SAXS data arrangement
=============================

After you reduce SAXS data, you will have in your Igor experiment data arranged in data folders also - in this case you will have SAXS data in root\:SAXS\:Samplename. These data use "QRS" naming :ref:`system <important.QRS>`.
To see inside of the current Igor experiment, use DataBrowser (ctrl-B or cmd-B).

.. image:: media/USAXSComputerDataArrangement.jpg
        :align: center
        :width: 480px



.. index::
    SAXS data reduction, Configuration

SAXS data reduction
===================

Data reduction for this instrument is done using  :ref:`Nika package <Introduction_Nika>`. You need to have Nika package :ref:`installed <Installation>`.
Select "Load Nika 2D SAS macros" from "Macros" menu, or preferably, load "USAXS, Irena and Nika" which will load all there packages. This will create "SAS2D" menu. Note, that it will take some time to compile the code, depending on the speed of your computer. Select from "Instrument Configurations" menu in SAS2D first item : "9IDC or 15IDD USAXS-SAXS-WAXS". This will create panel which can be used to configure Nika package to use on our instrument.

.. Figure:: media/SAXSReductionConfig.jpg
        :align: left
        :width: 500px
        :Figwidth: 820px

Select (or keep selected) checkbox "SAXS" and follow the instructions in the red letters. Keep other checkboxes selected as they are by default, more info later... First step is to push button "Set default settings". This will create dialog where you need to navigate to location of your SAXS data (see above about the data arrangement) and you need to select *any* data file from your samples, assuming there was no change in geometry for the data in that folder (distances, energy, etc.).
