.. _commonIssues:
.. _GUIcontrolsMissing:

.. index::
    Common Display issues
    Missing controls on Panels
    Panel artifacts

Common Issues
=============

Screen Resolution
-----------------

Summary: For best performance select 100% DPI setting ("Change the size of text, apps, and other items" in Windows 10) with less pixels used for display. Ideal Irena/Nika display seems to be HD TV resolution (1920 x 1080) or similar. Vertically, Irena and Nika need up to ~830 pixels for some panels.

**Explanation**:

When using Windows 7, 8, or 10, typically with high resolution displays (aka 4k, UHD, etc.), users often choose to set display number of pixels to high number but since the text and icons become small to read, they increase DPI - or as Windows 10 names it "Change the size of text, apps, and other items". This tells applications to scale up (if more than 100%) the windows so they can still be readable even with large pixel displays. Igor Pro 7 (at least 7.03 and below) does not handle this well enough - it seems that it can only scale panels up by full 100% steps (100%, 200%, 300%,...).

This means, that under some combination of **display resolution** (number of pixels) *and* **DPI settings** user can have the bottoms of the panels cut off and controls  which should be there are missing. Unluckily, even after working with Weavemetrics on this the only solution I know about is to modify display settings. Note, that future (as of 4/28/2017) versions of Irena and Nika will present users with error when they estimate that screen settings are incorrect.

Here is example of panel which is **missing bottom controls** due to incorrect settings.

.. image:: media/InCorrectScrRes.jpg
   :align: center
   :width: 380px


Here is how to get to the controls. Right click on the desktop (of the OS, not Igor Pro).

.. image:: media/W10AccToDPISett.jpg
   :align: center
   :width: 220px

Here is what you should see (again, Windows 10; Windows 7 and 8 are slightly different and call the setting *DPI*). Note the slider is moved to higher than 100% setting.


.. image:: media/W10HighDPISet.jpg
   :align: center
   :width: 450px

Here is modified setting which is 100% now:

.. image:: media/W10CorDPISett.jpg
   :align: center
   :width: 450px

And here is the same Igor panel with this setting, note the opresence of the **bottom controls**:


.. image:: media/CorrectScrRes.jpg
   :align: center
   :width: 380px

You may need to change now the display pixel resolution (numbers of pixels setting) to less pixels so you can actually read the text. Or get larger display.

Note, that it should be possible to use higher DPI settings with enough pixels on the screen. Above example was done with HD TV display setting (1920x1080 pixels). My display is 15 inch UHD (aka 4k) display, capable of displaying up to 3840 x 2160 pixels. But at that resolution it is basically humanly impossible to read anything. It is likely that I could use 4k setting AND 200% DPI setting, but I have seen some artifacts. Instead of raising the DPI to 200% I chose less pixels (HD resolutions) and 100% DPI. This has similar/same result with respect to size of text and icons, but Igor Pro works...
