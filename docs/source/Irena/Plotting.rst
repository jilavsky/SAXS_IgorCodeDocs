.. _irena-plotting-tools:
.. _Plotting_Tools:

.. index::
    Irena; Plotting tool I

Plotting tools
==============

List of tools
--------------

1. :ref:`Plotting tool I <plotting_tool_I>`
2. :ref:`Plotting tool II <Plotting_tool_2>`
3. :ref:`Multi Data Plotting tool <Plotting_tool_3>`

.. _plotting_tool_I:

Plotting tool I
---------------

.. Figure:: media/Plotting1.png
   :align: left
   :width: 280px
   :figwidth: 300px

This plotting tool produces publication-quality plots of 1D data: SAXS/SANS
and WAXS data, any Irena results, or any XY-type data. Simple fits are also
available. The default output is a standard XY plot, but the tool also supports
contour plots, waterfall plots, and Gizmo 3D plots, as well as movie creation
from 2D or 3D plots.

Plot user styles can be created and reapplied reproducibly across datasets.

.. note::

   Graph formatting is preserved only when applied through the tool's own
   panels.

Available operations:

1. Load and plot data; create new data types (e.g., Y × X\ :sup:`4`) automatically.
2. Modify data (scale, remove points, subtract background, etc.).
3. Simple fitting (Porod, Guinier, etc.).
4. Create 2D contour plots.
5. Create 3D plots — Waterfall (fast, simple) or Gizmo (slower, more powerful).
6. Create movies of 2D or 3D plots.
7. Save, import, and export graph styles.

This tool can also display Irena results such as size distributions and Unified
fits.

How to use
~~~~~~~~~~

Select "*Plotting I*" from the SAS menu.

The top section contains :ref:`standard data selection tools <DataSelection>`.
This tool can be scripted by the :ref:`Scripting tool <scripting_tool>`. Select
data and click "*Add data*". Multiple datasets can be added; the same dataset
cannot appear twice.

Apply a graph style from the "*Graph style*" popup, or select data types for
both axes manually. Required derived data (e.g., Q × Intensity) are created
automatically.

Use checkboxes and the "*Change graph details*" button (opens an expanded panel)
to modify graph appearance.

Example — log-log style applied to selected data:

.. Figure:: media/Plotting2.png
   :align: center
   :width: 100%

Axis labels support Igor formatting for subscripts, superscripts, and Greek
letters.

To set axis limits: zoom in Igor (select area, right-click), then choose
"*ZoomAndSetLimits*" from the context menu to apply zoom as fixed axis limits.

Scripting
~~~~~~~~~

Plotting Tool I can be scripted to add multiple datasets without manual
interaction:

.. Figure:: media/Plotting11.png
   :align: center
   :width: 400px

Use the scripting tool to either reset the Plotting tool and add files, or
append files to the existing plot:

.. Figure:: media/Plotting12.png
   :align: center
   :width: 100%

A time series of SAXS data plotted in 2D is often not very informative. The
following 3D options provide better representations.

Contour plot
~~~~~~~~~~~~

From version 2.52, contour plots are available. Load a series of data (best
via the Scripting tool) and click "*Contour plot*":

.. Figure:: media/Plotting16.png
   :align: center
   :width: 350px

Controls allow basic modifications: min/max contour values, number of contours,
labels, log spacing, color choice, and smoothing.

The contour plot is a standard Igor XY graph; additional customization can be
done using Igor's graph tools.

Waterfall 3D graph
~~~~~~~~~~~~~~~~~~

Click "*(Re)Graph (3D, Wf)*" to create a Waterfall graph:

.. Figure:: media/Plotting13.png
   :align: center
   :width: 100%

.. Figure:: media/Plotting14.png
   :align: center
   :width: 100%

Gizmo 3D graph
~~~~~~~~~~~~~~

The Gizmo tool (available from version 2.48) provides a more powerful 3D
visualization. At least 3 datasets are required.

Click "*Gizmo (3D)*":

.. Figure:: media/Plotting17.png
   :align: left
   :width: 350px

The data are resampled onto a grid (Q-space resampling). If "*Log X*" is
selected in the main panel, log(Q) is used; if "*Log Y axis*" is selected,
log(Intensity) is used. Estimated calculation time is shown — larger or denser
datasets take longer.

.. Figure:: media/Plotting18.png
   :align: left
   :width: 350px

"*Create 3D data set and plot*" — creates the 3D data and renders the Gizmo
plot.

"*Recreate 3D plot*" — reuses existing 3D data (faster, but data may be stale).

Grid lines, axis labels, and the data-order legend are user-configurable.
Color scale is shared with the Waterfall 3D graph.

Click "*Sync w/main panel*" after changes in the main panel to update the Gizmo
plot.

Fitting
~~~~~~~

.. Figure:: media/Plotting5.png
   :align: center
   :width: 100%

Click "*Fitting*" to open the fitting panel. Select the fitting range with
cursors, optionally enable "*Use errors*", and select the fitting function.

Click "*Guess fit parameters*" for starting guesses — this may not always
succeed, but a good starting point is essential for least-squares fitting.

.. Figure:: media/Plotting6.png
   :align: center
   :width: 100%

.. Figure:: media/Plotting7.png
   :align: center
   :width: 100%

Fit results are displayed as annotations in the graph. Use "*Remove Tags and
Fits*" to clean up.

Remove a data set
~~~~~~~~~~~~~~~~~

"*Kill graph, reset*" — clears all data and resets the tool.

"*Remove data*" — removes one dataset at a time from the plot.

Creating user styles
~~~~~~~~~~~~~~~~~~~~

When the graph looks as desired, click "*Save new graph style*" and provide a
name (automatically checked for uniqueness). Styles can be renamed via
"*Manage Graph details*".

Several predefined styles (Guinier, Porod, Zimm, etc.) are included from
version 2.38.

.. note::

   Linearization fits (fitting log or ln of Intensity vs log or ln of Q)
   require the data to be in log/ln form. This is currently not fully supported
   within Plotting Tool I due to display logic constraints. A separate tool for
   these fits may be added in a future release.

Import and export of styles
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use the "*Manage Graph details*" button to open the style manager:

.. Figure:: media/Plotting3.png
   :align: center
   :width: 100%

The left list shows styles inside the Igor experiment; the right shows styles
stored outside Igor (on disk). Use the copy buttons to transfer styles between
the two locations.

Modifying data
~~~~~~~~~~~~~~

Click "*Modify data*" to open the data modification panel:

.. Figure:: media/Plotting4.png
   :align: center
   :width: 100%

.. warning::

   The first time this panel is used on a dataset, a backup copy is created.
   Recovering the backup removes **all** modifications made to that dataset.

Select data, apply modifications using buttons and numeric inputs. Use cursor A
(round) to select a single point or a low-Q cutoff; use cursor B (square) for
a high-Q cutoff. "*Cancel*" resets corrections to defaults for the current
session (not to the original data).

Wave name length is limited to 30 characters including the ``q_`` prefix.

Storing graphs and exporting figures
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Figure:: media/Plotting8.png
   :align: center
   :width: 100%

Click "*Store and recall graph*" to open the graph storage panel.

Top buttons: save the graph as TIFF or JPEG.

"*Save Igor recreation macro*" — saves an Irena-specific recreation macro as a
string in ``root:Packages:StoredGraphs:``. Graphs stored this way remain under
full Plotting Tool I control when restored, unlike native Igor Pro recreation
macros.

"*Store Irena plotting tool graph*" — stores the current graph under the
specified name.

The listbox shows all stored graphs; use the buttons below to restore or delete
selected graphs.

"*More…*" — additional tools, including adding a linked d-spacing axis to the
top of the graph. Note that this axis is removed when the graph is recreated
(any change that forces graph recreation resets it).

.. Figure:: media/Plotting9.png
   :width: 45%
.. Figure:: media/Plotting10.png
   :width: 45%

.. note::

   Saving each graph used in a publication as a separate Igor experiment
   (via "*Export as pxp*") is strongly recommended. This preserves the graph
   with all its formatting and data in an immutable file.

Movie making
~~~~~~~~~~~~

Click "*Create movie*" to open the movie creation panel:

.. Figure:: media/Plotting15.png
   :align: left
   :width: 280px

Create sequences of 2D graphs by adding or replacing data between frames.
Controls adjust the appearance of each frame. The 3D graph option uses the
Waterfall graph; Gizmo has its own movie tool from WaveMetrics.

----

.. _Plotting_tool_2:

.. index::
    Irena; Plotting tool II

Plotting tool II
-----------------

This tool — a modification of a plotting tool developed by Dale Schaefer —
controls the currently active top graph. It is more flexible than Plotting Tool I
in some respects but has no built-in data management.

.. note::

   This tool is deprecated and should not be used for new work.

.. Figure:: media/Plotting19.png
   :align: left
   :width: 100%

Any control changed in this GUI is applied immediately to the top graph. Unlike
Plotting Tool I (which reapplies all formatting on each change), this tool
applies only the specific control that was changed. It is essentially an Irena-
styled interface to Igor's native graph controls.

----

.. _Plotting_tool_3:

.. index::
    Irena; Multi sample plotting tool

Multi-Sample Plotting tool
---------------------------

This tool plots many Irena/Indra/Nika data types quickly and easily, including
XY data (Q/Intensity/Error or results such as size distributions and PDDF) and
common SAXS linearization plots (Guinier, Kratky, Porod, and bioSAXS plots).

.. Figure:: media/MultiDataPlot1.jpg
   :align: left
   :width: 800px
   :figwidth: 720px

Data selection
~~~~~~~~~~~~~~

Familiarity with the data selection tools simplifies use of this panel. Full
details are in :ref:`Multi Data selection <DataSelectionMulti>`.

Data types:

* **USAXS** — data from the APS USAXS instrument.
* **QRS** — default SAXS/WAXS naming in Irena and Nika. See
  :ref:`QRS data type <important.QRS>`.
* **Irena results** — fits, size distributions, PDDF, diffraction peaks, etc.
* **Any** — enter a regular expression to identify the X, Y, and (optionally)
  error waves. The first matching wave is selected.

"*Start Fldr.*" — sets the starting folder for the search.

"*Folder Match (RegEx)*" — filters the folder list using a regular expression.

"*Invert?*" — inverts the filter (shows folders that do NOT match).

"*Sort Folders*" — orders the folder list by one of several methods.

.. note::

   Select a specific starting folder to avoid listing all data in a large
   experiment. For example, ``root:SAXS:Sample1_00033:`` shows only the
   datasets for that specific sample rather than thousands of entries.

Graph controls
~~~~~~~~~~~~~~

This tool can control any graph. A few Igor concepts are relevant:

Every graph has a *Graph Window name* — a unique Igor name used for internal
addressing (displayed in red in the tool). If the graph was created by this
tool, the name follows the pattern ``MultiDataPlot_XYZ``.

.. Figure:: media/MultiDataPlot2.jpg
   :align: left
   :width: 400px
   :figwidth: 390px

*Graph Title* is the user-visible string in the graph window title bar.

Use the "*Select Graph*" pull-down to target a different graph. The menu lists
graphs created by this tool, plus the current top graph. To control a graph not
created by this tool, make it the top graph and select it via "*Select Graph →
top graph*".

.. Figure:: media/MultiDataPlot3.jpg
   :align: left
   :width: 400px
   :figwidth: 390px

"*New graph*" — creates a new empty graph. Adding data when "*Graph window name*"
shows "none" also creates a new graph.

Adding data
~~~~~~~~~~~

Double-clicking a dataset name appends it to the target graph. Each dataset can
appear in a graph only once. **Double-click does not apply formatting.**

"*Append to selected graph*" — adds all listbox-selected datasets. **This
button applies formatting.**

Formatting graphs
~~~~~~~~~~~~~~~~~

**Predefined data types** (X-Y, Guinier, Kratky, etc.) — select from the
pull-down menu. Required derived data are created if they don't exist. This can
significantly change the displayed graph; create a new graph first if needed.

**Individual controls** — axis scaling, traces, legends, offsets, etc. Applied
to the currently controlled graph.

"*Apply Style*" — applies predefined axis and label styles without changing
the data.

"*Apply all formatting*" — applies all selected formatting at once.

"*Apply Formatting automatically*" — applies formatting automatically when data
are added.

"*Export as jpg*" / "*Export as tiff*" / "*Export as pxp*" — saves the
controlled graph as JPEG, high-resolution TIFF, or an Igor experiment file.

3D graphs
~~~~~~~~~

Use "*Create Image Plot*", "*Create Contour plot*", or "*Create Waterfall plot*"
to generate 3D representations from a series of datasets loaded in the tool.

Important notes:

1. These functions depend on which graph is currently selected (typically the
   top graph).
2. They create new copies of the data, detaching them from the originals.
3. Data are stored in ``root:MultiDataPlot3DPlots:``; close graphs and delete
   orphaned data there when no longer needed.
4. For image plots: data are rebinned to linear Q steps (images cannot display
   log-scale axes). Tick placement may appear irregular — adjust the number of
   ticks using the provided controls.
5. The vertical axis of image plots requires user input. The tool creates tick
   positions (adjustable) but expects the user to provide meaningful labels and
   units.

Examples — log-axis image (top) and linear-axis image (bottom):

.. Figure:: media/MultiDataPlot4.jpg
   :align: center
   :width: 100%

.. Figure:: media/MultiDataPlot5.jpg
   :align: center
   :width: 100%
