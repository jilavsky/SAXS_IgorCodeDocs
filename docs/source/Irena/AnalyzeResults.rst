.. _analyze_results:

Analyze results
================

.. index:: Analyze results


This tool is used to do analyze results from other, complicated tools, where this functionality does not fit in. Currently it supports only results from Size distribution tool. This tool is available in SAXS > Support tools and it replaces older "Evaluate Size distributions" which is currently available in "old stuff" with the same functionality.

Implemented models:

* Volume distribution
* Number distribution

.. Figure:: media/AnalyzeResultsSD1.jpg
        :align: left
        :width: 700px
        :Figwidth: 750px

**Selecting data**

Understanding data selection tools makes user life easier. In the Data selection part of the panel you need to define sufficiently the data you want to look inside. There is detailed description on how to use this widget system :ref:`Multi Data selection <DataSelectionMulti>`. Please refer to that page for details. This tool can use three types of data - USAXS, QRS (SAXS or WAXS) as well as Irena results (results saved by other Irena tools). All SAXS/WAXS data which DO NOT come from APS USAXS instrument use QRS naming system. Only if you have our USAXS data, you should use USAXS data type. For everyone else, use *QRS* naming system that is how your data came through ASCII importer or through Nika. For Irena results, there are two meaningful tools to be applied - Volume and Number Size distribution results.

You need to select *Start fldr* (e.g., "root\:SAXS\:") and data type using *Folder Match* (e\.g., "sub").

**Add data using double click** Add data using double click. Data are always added to the top graph as log-Intensity vs log-Q. For some (Guinier, Porod,...) the lower graph presents linearization plot. For some (Sphere) no linearization plot is presented.

Now, user can save results. Results can be saved in three ways using the three checkboxes on the panel:

* Results can be recorded in Notebook. This can be opened using *Get Notebook With Results* button.

* Waves containing resulting values - and text wave with folder name - in Igor folder (root\:NameDependingOnMethod). User can create table with those results using button *Get Table With results*. Also, user can manually graph any of those values as needed.

* Results can be saved in the folder where the data came from. In this case waves with fitted Int-Q are created and results are placed in wave notes. User can plot these using Irena plotting tools (these are Irena results type) and look through the wave note values later using *Metadata Browser*.


**Run as sequence**

User can select multiple data sets in the listbox, method to use, Q range to use, and way to store results and run same analysis method on sequence of the data. Note, that data are processed in the order (from top to bottom) they are displayed in the Listbox. It is really useful to order the processing in meaningful order (time, temperature, etc.) which then results in the tables being in suitable order.

*To display & further process* the results stored in the results folder, you can use :ref:`DataBrowser additions <DataBrowser additions>`.


**Using the tool**

Here is picture of tool while used:

.. Figure:: media/AnalyzeResultsSD2.jpg
        :align: left
        :width: 700px
        :Figwidth: 750px

Add data into the graph (double click), select range with cursors, check if you want Mercury intrusion porosimetry graph and hit "Calculate Results". Save results as needed. For multiple data sets, set all correctly for one, test and then select range fo data in listbox and use "Evaluate sequence" to run on many quickly.
