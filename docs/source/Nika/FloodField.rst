.. _nika-flood-field:

.. index:: Nika; Flood field

Flood field
===========

This tool creates a pixel sensitivity (flat-field) correction map, called a
"Pix2D sensitivity field" in Nika. It was substantially upgraded in version
1.51.

**Definition:** The Pix2D sensitivity field is a matrix of real values such
that when the measured intensity in any pixel is *divided* by its Pix2D value,
the result is the corrected intensity::

    Corrected value = Measured value / Pix2D sensitivity value

To construct this pixel sensitivity map, all pixels must be exposed to the same
incident photon (or neutron) flux — a flood field or flat field. With sufficient
counts per pixel and a truly flat incident field, the measured intensity per
pixel is inversely proportional to pixel sensitivity.

If such an image is available, this tool converts it into a Nika-compatible
Pix2D sensitivity image. The image is saved as a TIFF file with single-precision
floating-point values per pixel. This format may not be readable by other
software packages.

To create the Pix2D map, each pixel value is divided by the maximum intensity
found in the image. If any pixel has zero intensity (which would cause a
divide-by-zero and permanently exclude that pixel from all future calculations),
an offset can be added to all pixels before the correction is applied.

GUI
---

.. Figure:: media/FloodField1.png
   :align: center
   :width: 380px

The interface is similar to the Mask GUI.

"*Select path to data*" — Navigate to the folder containing the flood field
image. The resulting Pix2D sensitivity file is saved to the same location.

"*File type*" — Select the file format of the flood field image. Note that the
resulting Pix2D sensitivity file is always saved as a TIFF.

.. note::

   This tool processes one file at a time. To average multiple flood field
   images, use "*Append file to image*" to accumulate intensities into a single
   combined image. If no 2D image exists yet, a new one is created. To start
   over with a fresh image, use "*Make image*".

"*Select data set to use*" — Select the image to use.

"*Make image*" — Loads the data and creates the 2D image.

"*Add value to each point*" — When selected, adds a constant offset to all
pixel values before processing. This reveals an additional "*Offset to add to
each point*" control. The offset is selected automatically if zero-valued pixels
are detected during loading, and deselected automatically if the minimum pixel
value is already above zero.

"*Use Mask*" — Applies the mask loaded in the main Nika tool when computing the
pixel sensitivity map.

"*Maximum value found in image*" and "*Minimum value found in image*" —
Informational displays. The maximum value can be overridden by typing a new
value; the entered value is then used in place of the detected maximum when
computing the correction.

.. warning::

   The code does not validate the entered values. For example, setting a
   negative offset can produce a negative Pix2D sensitivity map, which will
   cause errors in all subsequent data processing.

Example output:

.. Figure:: media/FloodField2.png
   :align: center
   :width: 100%

Left: control panel. Center: loaded flood field image. Right: computed Pix2D
sensitivity map after masking and scaling by the maximum intensity. The images
update as panel values are changed; if they do not, use the "*Display ...*"
button to force a refresh.

"*Save 2D pix sensitivity file (flood)*" — Processes and saves the file as
described above, appending ``_flood.tif`` to the name entered in the
"*Save as*" control. The name is checked for OS compatibility before saving.
