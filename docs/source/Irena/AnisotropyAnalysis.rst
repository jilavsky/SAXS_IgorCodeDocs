.. _AnisotropyAnalysis:

.. index::
   Anisotropy Analysis
   Hermans Orientational parameter

Anisotropy Analysis - HOP
=========================

**This tool analysis anisotropy of oriented peak using Nika generated intensity vs azimuthal angle (r-wave vs az-wave) according for formula 8 in : P. C. van der Heijden, L. Rubatat, O. Diat, Macromolecules 2004, 37, 5327.** see:  https://pubs.acs.org/doi/10.1021/ma035642w

For more info check  L.E. Alexander, R.J. Roe, etc.

This tool uses angular dependence of scattered intensity at some specific q value. Simplest method to get suitable input data for this tool is top use  Nika and use :ref:`Line profile <LineProfileTool>` tool to generate intensity profile along *circle* at whatever q value for your peak is - and this way you get intensity as function of azimuthal angle. Nika stores the data as triplet of waves az_Dataname, r_Dataname, and s_Dataname; where az_Dataname is azimuthal angle in degrees. It is strongly suggested, that you properly calibrate and background subtract the data or the results of these calculations will be wrong.

Main GUI
--------

This is the main screen:

.. Figure:: media/AnisotropySys1.jpg
   :align: center
   :height: 480px

In the top part are :ref:`standard data selection tools <DataSelection>` . Data can be selected using standard Irena selection system. You can use Nika generated pair of  az_dataName and r_dataName (uncertainty wave is optional) when checkbox "Nika Az data" is checked or arbitrary named data if it is unchecked. Note, that you MUST provide data for azimuthal angle **in degrees**. Select data and push “\ **Graph data**\ ”  button. Graph of data is generated:

.. Figure:: media/AnisotropySys2.jpg
   :align: center
   :width: 580px


Select the peak area with cursors - set cursors below half intensity point on each side of the peak. The maximum peak must be reasonably far from 0/360 degrees. In other words, you cannot have the peak split *exactly* by 0/360 degrees value. Internally data are extended to either side, if needed, so you can be relatively close to 0 or 360, but you need to have enough of the peak in view to get reasonable peak position fit. Select suitable Peak profile shape (Gauss or Lorenzian) and push button "Fit Peak". Code will run through complete analysis and fill the table with results:

.. Figure:: media/AnisotropySys3.jpg
   :align: center
   :width: 680px

Results are in the table as well as in the graph itself.

Button “\ **Save results (notebook)**\ ”  will save results with graph in Irena :ref:`Results notebook <ResultsNotebook>` . You can save this notebook as rtf file and use it in any word processor.

There are no other data to save or export and this tool cannot be scripted for now. Other tools and options can be added in this tool, if you know about any, let me know.
