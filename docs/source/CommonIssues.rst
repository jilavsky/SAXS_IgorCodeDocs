.. _commonIssues:
.. _GUIcontrolsMissing:

.. index::
    Reading Igor 9 saved experiments in Igor 8
    Reading HDF5 files with non standard compression
    Common Display problems
    Missing controls on Panels
    Panel artifacts
    Error caused by missing HDF5 xop - Igor 8 ONLY
    xop not loading (macOS)

Common problems
===============


Reading Igor 9 saved experiments in Igor 8
------------------------------------------

Igor 9 added option to scale Panels with scaling factor which was not available in Igor 8. This method is much better than what Irena and Nika are using in Igor 8 and before. However, Igor 8 does not understand the flag used to scale the panels and if you try to open Igor 9 saved experiment with opened panel in Igor 8, you get error on start. The solution here is to use leftmost button on dialog "Quit macro" (not "Abort experiment load" !), acknowledge what Igor wants you to know and then reopen the panel you are missing. The "Load Experiment Diagnostic" window can be dismissed, it is not really useful in this case. Also note, that to prevent data loss, Igor will not name this experiment by its original name, you may need to give it meaningful name before saving.

Reading HDF5 files with non standard compression
-------------------------------------------------

HDF5 files (e.g., Nexus) can use compression to save space. Standard in HDF5 is GZIP, which is available in Igor by default. However, there are others (e.g., LZ4) used by specific software or devices (Dectris Eiger/Pilatus detectors use LZ4). Igor 8 and 9 on Windows and Igor 9 on macOS can be relatively simply extended using plugins to read compressed data using these additional compressions. Instructions are here: https://www.wavemetrics.com/comment/22658#comment-22658 . Basically, you need to download plugins from links provided and place them in default location on each system. HDF5 in Igor will then pickup the plugins when needed and be able to READ these compressed data.

Screen Resolution
-----------------

Summary: For best performance select 100% DPI setting ("Change the size of text, apps, and other items" in Windows 10) with less pixels used for display. Ideal Irena/Nika display seems to be HD TV resolution (1920 x 1080) or similar. Vertically, Irena and Nika need up to ~830 pixels for some panels.

**Explanation**:

When using Windows 7, 8, or 10, typically with high resolution displays (aka 4k, UHD, etc.), users often choose to set display number of pixels to high number but since the text and icons become small to read, they increase DPI - or as Windows 10 names it "Change the size of text, apps, and other items". This tells applications to scale up (if more than 100%) the windows so they can still be readable even with large pixel displays. Igor Pro 8 does not handle this very good - it seems that it can only scale panels up by full 100% steps (100%, 200%, 300%,...). Igor 9 is much better in handling different screen resolutions.

This means, that under some combination of **display resolution** (number of pixels) *and* **DPI settings** user can have the bottoms of the panels cut off and controls  which should be there are missing. Unluckily, even after working with Wavemetrics on this the only solution I know about is to modify display settings. Note, that future (as of 4/28/2017) versions of Irena and Nika will present users with error when they estimate that screen settings are incorrect.

Here is example of panel which is **missing bottom controls** due to incorrect settings.

.. Figure:: media/InCorrectScrRes.jpg
   :align: center
   :width: 380px


Here is how to get to the controls. Right click on the desktop (of the OS, not Igor Pro).

.. Figure:: media/W10AccToDPISett.jpg
   :align: center
   :width: 220px

Here is what you should see (again, Windows 10; Windows 7 and 8 are slightly different and call the setting *DPI*). Note the slider is moved to higher than 100% setting.


.. Figure:: media/W10HighDPISet.jpg
   :align: center
   :width: 450px

Here is modified setting which is 100% now:

.. Figure:: media/W10CorDPISett.jpg
   :align: center
   :width: 450px

And here is the same Igor panel with this setting, note the presence of the **bottom controls**:


.. Figure:: media/CorrectScrRes.jpg
   :align: center
   :width: 380px

You may need to change now the display pixel resolution (numbers of pixels setting) to less pixels so you can actually read the text. Or get larger display.

Note, that it should be possible to use higher DPI settings with enough pixels on the screen. Above example was done with HD TV display setting (1920x1080 pixels). My display is 15 inch UHD (aka 4k) display, capable of displaying up to 3840 x 2160 pixels. But at that resolution it is basically humanly impossible to read anything. It is likely that I could use 4k setting AND 200% DPI setting, but I have seen some artifacts. Instead of raising the DPI to 200% I chose less pixels (HD resolutions) and 100% DPI. This has similar/same result with respect to size of text and icons, but Igor Pro works...


.. _HDF5xopError:

.. index::
    HDF5 error
    Missing xop error
    HDF5OpenFile error


Error caused by missing HDF5 xop - Igor 8 ONLY
----------------------------------------------

This is error specific to Igor 8 and before, Igor 9 does have HDF5 functions built in... This error appears when Installer does not make proper link to Igor Pro included HDF5.xop or for some other reason this library is not loaded properly on Igor start. You will see something similar to:

.. Figure:: media/HDF5xopError.jpg
   :align: center
   :width: 380px

Important here is that you see error on line containing HDF5Open... HDF5Close... or any other error related to HDF5 etc. This is due to missing link/alias to the xop library or the library not being properly loaded. To learn more about Igor Extensions, run in Igor command prompt: *DisplayHelpTopic "Igor Extensions"*

Here is how you fix this problem:

1.  If you just installed Irena/Nika/Indra, you need to **quit** Igor Pro and start it again; only creating New Experiment is not enough. These xop packages are loaded when Igor starts. So this HDF5.xop may not be loaded.

2. If that does not work, you need to manually create shortcuts (Windows) or alias (OSX) between following files and locations. Note: Use aliases (shortcuts, links) and do not simply copy the files, with aliases, if you upgrade Igor to new version in the future, HDF5 library will be upgraded also.  During Igor upgrade the alias/Link target will be upgraded by Igor installer. Note, *HDF5.xop* is 32 bit version of the executable package, *HDF5-64.xop* is 64 bit version of executable package, and *HDF5 Help.ihf* is help file.

3. (A) 32 bit versions of Igor Pro (Igor 6.37):

*  Applications(OSX) or Program Files(win)/Igor Pro 6 Folder/More Extensions/File Loaders/*HDF5.xop*    ---  alias/link to --- Documents/Wavemetrics/Igor Pro 6 User Files/Igor Extensions/ *place alias here...*

*  Applications(OSX) or Program Files(win)/Igor Pro 6 Folder/More Extensions/File Loaders/*HDF5 Help.ihf*    ---  alias/link to --- Documents/Wavemetrics/Igor Pro 6 User Files/Igor Extensions/ *place alias here...*

   (B) 64 bit version of Igor Pro (7.x or 8.x)

*  Applications(OSX) or Program Files(win)/Igor Pro 7(or 8) Folder/More Extensions (64-bit)/File Loaders/*HDF5-64.xop*    ---  alias/link to --- Documents/Wavemetrics/Igor Pro 7(or 8) User Files/Igor Extensions (64-bit)/ *place alias here...*

*  Applications(OSX) or Program Files(win)/Igor Pro 7(or 8) Folder/More Extensions/File Loaders/*HDF5 Help.ihf*    ---  alias/link to --- Documents/Wavemetrics/Igor Pro 7(or 8) User Files/Igor Extensions/ *place alias here...*


Quit Igor Pro, restart and it should work now correctly. If not, please contact me so I can identify the problem.


xop not loading (macOS)
-----------------------

Please note, that macOS Catalina and later versions have issues loading old (unsigned) xop packages due to system protection system (Gatekeeper). I personally run Igor 8.04 and 9.00 on Catalina without problems, but getting xops to load first time is bit challenge. One time challenge... If you need to use Catalina or later, here are some helpful links. General Wavemetrics statement macOS xop load issue: https://www.wavemetrics.com/news/igor-pro-macos-1015-catalina , and how to get xops loading https://www.wavemetrics.com/forum/general/workaround-catalina-xop-problem.
