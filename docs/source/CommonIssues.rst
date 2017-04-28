.. _commonIssues:
.. _GUIcontrolsMissing:

.. index::
    Common Display issues
    Missing controls on Panels
    Panel artifacts


Screen Resolution
=================

When using Windows 7, 8, or 10, typically with higher resolution displays (aka 4k, UHD, etc.) users often choose to set display number of pixels to high number but since the text and icons become small to read, they increase DPI - or as Windows 10 names it "Change the size of text, apps, and other items". This tells applications to scale up (if more than 100%) the windows so they can still be readable. Igor Pro 7 (at least 7.03 and below) does not handle this well enough - it seems that it can only scale panels up by full 100% steps (100%, 200%, 300%,...).

This means, that under some combination of display resolution (number of pixels) and DPI settings user can have the bottoms of the panels cut off and controls  which should be there are missing. Unluckily, even after working with Weavemetrics on this the only solution I know about is to modify display settings. Here is example of panel which is missing bottom controls due to incorrect settings.

.. image:: media/InCorrectScrRes.jpg
   :align: center
   :width: 380px
   :figwidth: 320px


Here is how to get to the controls. Right click on the desktop (of the OS, not Igor Pro).

.. image:: media/W10AccToDPISett.jpg
   :align: center
   :width: 220px
   :figwidth: 200px

Here is what you should see (again, Windows 10 - Windows 7 and 8 are similar). Note the slider is moved to higher than 100% setting.


.. image:: media/W10HighDPISet.jpg
   :align: center
   :width: 380px
   :figwidth: 350px

Here is modified setting which is 100% now:

.. image:: media/W10CorDPISett.jpg
   :align: center
   :width: 380px
   :figwidth: 350px

And here is Same Igor panel with this setting:


.. image:: media/CorrectScrRes.jpg
   :align: center
   :width: 380px
   :figwidth: 320px

You may need to set now display pixel resolution (numbers of pixels setting) to less pixels so you can actually read the text. Or get larger display.

Note, that it shoudl be possible to use higher DPI settings with enough pixels on teh screen. ABove example was done with HD TV display setting (1920x1080 pixels). My display is UGH (aka 4k) display, capable of displaying 3840 x 2160 pixels. But at that resolution it is humanly impossible to read anything. Instead of raising the DPI to 200% I chose less pixels and 100% DPI.
