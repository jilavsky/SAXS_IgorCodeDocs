.. _reduce_data_procedure:
.. _reduce_data_panel:

.. index::
    Reduce USAXS data, Introduction

Reduce USAXS Flyscan data
-------------------------

**Comments:**

When using Indra package to reduced data, it uses "USAXS" naming system for use with Irena macros. When you use Irena to analyze, plot, or even only export data, you need to select "USAXS" choice in the top of the panels.

**Make sure you save Igor experiment**
Igor is quite reliable and crashes rarely. But no software is crash proof and you really do not want to loose lot of your work and time. Therefore, do yourself a favor and save your Igor experiment, routinely.
**YOU WERE WARNED!!!!**


.. index::
    Collected data arrangement

Collected data arrangement
==========================

When you collect data on 9IDC USAXS/SAXS/WAXS instrument, your data are saved in folders related to your "spec" file name. Spec file itself is text file where instrument makes various records. The file name is created by adding \MM_DD_ (\month_day_) to the name staff selects, typically version of user name. When you collect USAXS data, a folder with the same name with appended "_usaxs" is created. For SAXS data we create folder with the same name with "_saxs" and for  WAXS with "_waxs". See below in the Image:

.. Figure:: media/USAXSComputerDataArrangement.jpg
        :align: center
        :width: 480px

After you reduce data from USAXS instrument, you will have in your Igor experiment data arranged in data folders also - in this case you will have USAXS data in root\:USAXS\:Samplename. Similarly SAXS data will be in folder root\:SAXS\:Samplename and WAXS in root\:WAXS\:Samplename. If you did not collect one or more of these segments of our data, the folder may not be present or it may be empty.

Assuming you did use scripts created by automatic routine or the manually typed commands in the script file were properly in order, there will be same number of USAXS, SAXS, and if measured WAXS files, with same names and important, same "order numbers" = end numbers "_XYZW" attached by the instrument, are the same for measurements which belong together. This is important in later data merging.

Reduced USAXS data arrangement
==============================

To see inside of the current Igor experiment, use DataBrowser (ctrl-B or cmd-B). Your data from USAXS instrument (not SAXS or WAXS) will be in folder root\:USAXS\:Sample_name_Folder

.. Figure:: media/USAXSIgorDataArrangement.jpg
        :align: center
        :width: 480px

In the Figure above the "waves" are sorted in the order they are created. You can ignore most of them, but two sets:
  1.  SMR_Int, SMR_Qvec, SMR_Error, and SMR_dQ. These are SLIT SMEARED version of your data. These are created first and are (usually ) absolutely calibrated, fully corrected Intensity-q-error-resolution data. These are used by Irena for modeling.
  2.  DSM_Int, DSM_Qvec, DSM_Error, and DSM_dQ. These are present if you desmear the data. These are pinhole equivalent data which can be used by ANY data analysis software.


.. index::
    Reduce USAXS data, Flyscans
.. index::
    USAXS data reduction, Flyscans

Reduce Flyscan data procedure
=============================

This chapter walks reader through very simple (basic) reduction of USAXS data collected using "Flyscanning". This is the most common method of data collection for the USAXS instrument and if you were NOT told you used step scanning method, you probably used Flyscanning. You can find movies of this procedure in my Youtube channel, so if you prefer to watch movie, check there. If you prefer text and pictures, here is simple way or reducing (USAXS & SAXS & WAXS) data, including merging them together.

Flyscanning is the most common method of data collection for the USAXS part of the USAXS/SAXS/WAXS instrument. If you were NOT told you used step scanning method, you probably used Flyscanning. *If you collected data using step scanning, see separate chapter.* Following this chapter on USAXS data reduction will be chapter on SAXS and then WAXS data reduction. Followed by merge data procedure. Note, that SAXS and WAXS data reduction uses Nika package and merging uses Irena package.

>> *If you collected data using step scanning, see separate chapter.* <<


Select "Load USAXS macros" from "Macros" menu. This will create "USAXS" menu and also open "Read me" notebook. Note, that it will take some time to compile the code, depending on the speed of your computer. Select "Import and reduced USAXS data" from the "USAXS menu".

.. Figure:: media/USAXSDataReduction1.jpg
        :align: left
        :width: 800px
        :Figwidth: 820px

Follow these steps:

Use “\ *Select data path”* to browse to the folder on the computer where the USAXS data are. In my test case this is folder ".../TestData/Test_usaxs"

.. Figure:: media/USAXSDataLocation.jpg
        :align: left
        :width: 400px
        :Figwidth: 420px

First we MUST process instrumental curve = "Blank" (aka "Empty" or similar names). This is important to do FIRST since without having proper instrumental curve, we cannot reduce and calibrate data measured on any sample. It is critical to use Blank measurement collected with EXACTLY the same setup, same energy, and as close in time to sample measurement as reasonable. Weaker the scattering, more important is to have a good Blank. Note, if your sample is inside environment (capillary, heater,...) the Blank includes the environment. For capillaries one can have two types of Blanks - empty capillary OR solvent. Talk to staff which one is appropriate for your specific case. If in doubt, collect both and decide later...

Make sure the checkbox "Process as Blank" is checked and Blank sample measurement is highlighted in the *List of available files”* listbox. Push button *Load/process one*

.. Figure:: media/USAXSDataReduction2.jpg
        :align: left
        :width: 800px
        :Figwidth: 820px

In the main graph you see Intensity-vs-q plot (log-log). In the top right corner is inset of the same Intensity data, but plotted against angle of analyzer stage. It is fitted at the top with Gauss+Lorenz function which provides center (angle at which q=0), width of the rocking curve (this is q resolution and is needed for absolute calibration) and maximum at the top (this is needed for absolute calibration). If this fit in the inset does not look good enough, move cursors up/down and try fitting with the buttons yourself. If this keeps failing, talk to beamline scientist to get help. The main graph shows how the instrument profile looks like. These profiles vary based on crystal surface quality and various dimensions in the instrument.

Sometimes you need to make sure diode gains are aligned correctly, see the Tab  *Diode* discussion below.

Push button *Save Data* and this blank curve will be stored in way the code can use it in next steps. Uncheck the *Process as Blank* checkbox and on the pulldown menu *Blank Folder* which appears below the Listbox with *List of available data* pick the name of Blank you created. Select a sample in the listbox and push button *Load/process one*. You should see something like this:

.. Figure:: media/USAXSDataReduction3.jpg
        :align: left
        :width: 800px
        :Figwidth: 820px

What you see here is presentation of measured data (scaled by 1/transmission) - red curve - with Blank - black curve - plotted against left axis. You see subtraction - blue curve - plotted against right axis. This is Subtracted, calibrated, slit-smeared data. In the inset you should see fit to the peak profile of intensity vs angle plot, again providing values for q=0 angle, maximum intensity and width of the rocking curve.

Now we will check/modify some things in the tabs. Follow this procedure:

Tab *Sample*
  1.  In the main panel in the tab *Sample* (it should be the top one) check that calibration method is "Calibrate [cm2/cm3]"" if you have meaningful sample thickness. If data will not be calibrated at all, check "Calibrate Arbitrary" and if you have powders and need absolute calibration in units/weight, talk to beamline staff how to do this right. It gets complicated...
  2.  Make sure the thickness is right. If this was provided at the data collection time, it should be. If you need different thickness, you can overwrite. If you have many samples with same - and different than you used during collection - thickness, you can write the number into "Overwrite Sample Thickness" and it will be used for all subsequent samples.
  3.  Transmission settings should be correct. There are multiple measurements of transmission in the USAXS and if all of them are within 5-10% of each other, all should be fine. If there are significant variations, talk to staff.
  4.  *FlyScan rebin to* We collect 8k points over the angular range. That is too much for analysis. For regular (smooth) USAXS data 200-400 points of whole range is more than enough. If you have sharp features - diffraction peaks, Bessel function oscillations - you may need to increase the number to 600-1200 points. Note that, logically, the noise increases as you increase number of points due to simple statistical reasons.

Tab *Diode*
  1.  Most numbers here do not need changing, except the "Background 5" sometimes. If the measurement of electronic background and diode dark current is for some reason different significantly between sample and Blank - or if your sample has high absorption, you may find the sample and Blank data crossing at high q. In that case reduce the value in "Background 5" to half or even less of measured value. If you have to change that for each sample, place overwrite value in "Overwrite Background 5" field. Correctly there is some flat background left in the data after the subtraction.
  2.  Check the colored segments in the main graph now on the main graph. These different colors indicate different gains of amplifier and sometimes the changes between them are not fast enough and removed by our code. If that happens, you can check the checkbox *Remove Flyscan dropouts?* at the bottom of the panel and if needed, increase *Drpt. time* value (I have seen up to 1 second). The other values are usually not needed, but if needed, can be changed also. This tool should removed the transitional points where intensity is collected with incorrect gain records.

.. Figure:: media/USAXSDataReduction4.jpg
        :align: left
        :width: 800px
        :Figwidth: 820px

Tab *Geometry* Ignore this tab, any changes here are NOT going to help you.

Tab *Calibration* Ignore this tab, any changes here are NOT going to help you.

Tab *MSAXS* Ignore this tab, any changes here are NOT going to help you.

Tab *Desmear*
  If you plan to use ANY other tool than Irena package for data analysis - anything else, including simply plotting and fitting with power law etc., you MUST desmear the data. As of now, I am not aware of ANY package for analysis of SAS data which would know how to fit our slit smeared data reliably. To desmear data, check checkbox *Desmear Data*.
  Then decide what extension function *Background function* you need - often the flat is correct, sometime, like here, you need "Power-law with flat". You can see the results of fitting in the main graph, it is the red dotted line in lower right corner. Ideally it fits well data at high q - typically above q=0.1 A^-1. If needed, change the fitting function and/or the *Background extrapolation start*

.. Figure:: media/USAXSDataReduction5.jpg
        :align: left
        :width: 800px
        :Figwidth: 820px

Note that now there are two versions of your subtracted (and calibrated data). One version is the blue curve - this is slit smeared USAXS data. The there is green version of the same curve - this is desmeared version of the data. The desmeared version of the data is version you can model with ANY fitting program for SAS data analysis. Slit smeared data can be modeled ONLY with Irena package.

Ignore most other stuff in the graph - the little dots are normalized residuals which we get if we slit smear the desmeared data and compare them with original slit smeared version. Ideally these are randomly distributed between +1 and -1. There are no controls in this desmearing tool, so if you need to handle cases where this routine does not work well enough, you need to save only slit smeared data and use dedicated package in irena, where you have a lot more controls. Note, that desmearing often (always) adds noise to the data,. Desmeared version will ALWAYS be more noisy. If you have noisy data to start, desmearing may make them unusable. If you plan to use Irena, there is no major reason to desmear the data, expect for presentation purposes. Irena has slit smearing of model built in.

Important - Qmin range - check
==============================

**This is critical and important! - this is also SAMPLE SPECIFIC and each sample (or range of samples) may need different Qmin**

1.    It is critical to set the rounded cursor on the main graph (cursor "A") correctly. This is sample dependent - the rounded cursor on the log-log Intensity vs q curve defines starting point in which we start with data subtraction. Note, that instrumental curve is raising at low-q values around Q^-8 or so. With this steep raise there can be observable linear difference in intensity, which has very high uncertainties. In the above graphs the round cursor is set to instrument resolution, but sample scattering at that q is weak. While the data look OK, their reliability is probably not very good. User needs to correct this and select starting point, where the sample intensity clearly deviates from instrumental background curve. This varies sample-per-sample. This is important USER FUNCTION and no code can handle this for users. In this case we need to move cursor few points higher to make sure the data we are getting are reliable and robust. You want there be clearly observable difference between sample and blank where the cursor is... See below.
2.    Check for multiple scattering. Many samples (mainly powders) exhibit multiple scattering. Complicated for this place, but you need to check and if needed, ask staff. Samples will exhibit multiple scattering if the FWHM (full width of half maximum) of the peak profile fit for sample is significantly wider than Blank. If it is more than 20% wider, ask. At this energy (21keV) the FWHM for Blank and this sample are both ~2 arc seconds, so in this case if sample is 2.4 arc second or more, **ask, ask!**. FWHM is energy dependent, it may be different if you collect data at other energies.
3.    If you see "Warning - too small Qmin detected. Reset to calculated Qmin = something", the starting point (round cursor) is too much left from calculated instrument resolution. It was moved right. This happens ONLY when you *Load/process one*
4.    NOTE: position of round cursor is remembered between samples, it is never moved left, only right when needed. You may need to check its position for each sample, as the right starting condition depends on strength of sample scattering at various q values.

.. Figure:: media/USAXSDataReduction6.jpg
        :align: left
        :width: 800px
        :Figwidth: 820px

Here is processed data set. When happy, push button *Save Data* and data are saved. Note, a new graph is created, and in this graph presents Intensity vs q curve, desmeared one if you were desmearing, and slit smeared one if not. You can kill this graph, it will be recreated if needed...


.. Figure:: media/USAXSDataReduction7.jpg
        :align: left
        :width: 800px
        :Figwidth: 820px


You can process next sample/s.
