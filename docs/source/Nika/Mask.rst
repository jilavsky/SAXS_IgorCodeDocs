.. index:: Mask Nika

Mask
====

.. Figure:: media/Mask1.png
   :align: center
   :width: 100%


To create mask use tool “Create mask” from either “SAS 2D” or Mask tab on main panel. Select Figure which you want to use to build the mask from.

Push “Make Figure” to create Figure. Modify by Display log display checkbox, sliders and Colors popup to see details on the Figure as necessary. Then push “Start MASK draw” to start drawing mask. This brings tools up on the graph.

.. Figure:: media/Mask2.png
   :align: center
   :width: 100%


Use Igor tools (rectangle and other tools) to cover area to be masked OFF. You can use zoom in and out to see some specific area of the Figure and draw with higher precision, but you need to switch between drawing tools and graph tools in the tool on the left hand side of the graph. Note, that on mac sometimes you can have the tools hidden behind the Panel.

.. Figure:: media/Mask3.png
   :align: center
   :width: 100%


If first/last few lines need to be covered, use the “Mask first/last columns/rows” input fields. This will add rectangles to objects drawn in the Figure:

.. Figure:: media/Mask4.png
   :align: center
   :width: 100%

Next, if the Figure contains some areas, which have low intensity and contain no information like in this MarCCD Figure, where the CCD is rectangle, but scintillator material is circle, you can mask off points with “Mask low intensity points” and setting threshold value. NOTE: this value depends if you are using directly Figure or if you are using log(Figure). In this case the corners have values set to 0 and so we can mask off anything which is less than may be 3 or so. Figure has dark counts/background of may be 100, so it is safe to remove all low intensity points.

.. Figure:: media/Mask5.png
   :align: center
   :width: 100%


OK, now we should have suitable mask for this case. You can now push button “Finish MASK” and the Figure will be set to regular mode. To continue drawing you need to push button “Start MASK draw” again. Previous drawing will NOT be removed.

Then we can SAVE the mask, giving it easy to understand name. Code will add \_mask.hdf to the name. This mask is now available to the code and also can be loaded later from hdf file as mask to be used.

NOTE: prior version 1.49 Nika used Tiff file for mask storage. It can now load either the tiff file or the hdf file, but it will now save only hdf file, for reasons explained below…

**Editing old mask:**

I have been asked multiple times to enable editing of existing mask. Using the tiff file this is not possible, so from version 1.49 Nika will use hdf file. This file now stores both Figure to be used in analysis by Nika as well as recreation macro to be used by “Create Mask” tool. Therefore, one can now store partial masks and combine them later into meaningful combination. Therefore, if there is known mask related to dead or bad pixels on detector, one can store that separately and then use it and always add parts of mask needed for specific setup. Here is any example…

Starting with Tif Figure and nothing:

.. Figure:: media/Mask6.png
   :align: center
   :width: 100%


This is the beamstop:

.. Figure:: media/Mask7.png
   :align: center
   :width: 100%


This is the stick:

.. Figure:: media/Mask8.png
   :align: center
   :width: 100%


And here are the low-intensity points.

.. Figure:: media/Mask9.png
   :align: center
   :width: 100%


In each case I “Started MASK draw”, made my choices by drawing objects or modifying some settings, “Finished MASK”, and then gave the mask a name and saved it. The masks have extension .hdf and can be seen in the same place where the data are if you select File type hdf;

.. Figure:: media/Mask10.png
   :align: center
   :width: 100%


Now you can create new Figure using your data and load sequentially the three masks we just created - creating new more complex composite mask. You can also add more drawings to is as you see fit and then save complete “full” mask… Select one file after another in the list box and push button “Load existing mask”.

Here are Figures with original, adding beamstop\_mask.hdf, Stick\_mask.hdf and then the LowIntPoints\_mask.hdf:

.. Figure:: media/Mask11.png
   :width: 45%
.. Figure:: media/Mask12.png
   :width: 45%

.. Figure:: media/Mask13.png
   :width: 45%
.. Figure:: media/Mask14.png
   :width: 45%

And then I save this mask as full\_mask.hdf which I then use in real work…

.. Figure:: media/Mask15.png
   :align: center
   :width: 100%
