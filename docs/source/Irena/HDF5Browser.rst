.. _irena-hdf5-browser:

HDF5 Browser
============

The HDF5 Browser is a custom Igor Pro tool for exploring, browsing, and
transferring data between HDF5 files and Igor Pro experiments. It provides a
Data Browser-style two-pane interface where each pane independently displays
either an HDF5 file or the current Igor experiment's data folder hierarchy.

**Key features:**

- **Two independent panes**: simultaneously browse two HDF5 files, or one HDF5
  file and the Igor experiment
- **Collapsible tree navigation**: expand/collapse groups to navigate hierarchies
- **Value preview widget**: inline display of scalar values, text strings, and
  previews of 1D/2D arrays
- **Attribute display**: view metadata (HDF5 attributes, Igor wave notes, folder
  attributes)
- **Copy operations**: transfer datasets/groups between panes and into/out of
  Igor Pro
- **Drag-and-drop**: intuitively move or copy items using mouse drag
- **Right-click context menu**: quick access to copy, inspect, plot, and folder
  management functions
- **Attribute preservation**: round-trip HDF5 attributes through Igor experiment
  exports/imports via wave notes and sidecar waves

File types and data sources
---------------------------

Supported HDF5 file formats
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The browser accepts standard HDF5 files and Igor experiment files (which can
embed HDF5):

- **.h5** — Standard HDF5 file format
- **.h5xp** — Igor Pro experiment file containing HDF5 data (read/write)
- **.pxp** — Igor Pro experiment file (readable as HDF5 via Igor's native HDF5
  interface)

Igor experiment as data source
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Each pane can also browse the **current Igor experiment** directly by clicking
the "*Igor*" button. This displays:

- **Folders** — mapped from Igor data folders
- **Waves** — individual Igor waves (numeric or text)
- **Variables** — scalar numeric values
- **Strings** — text variables
- **Attributes** — wave notes and folder sidecar attributes (see Attribute
  preservation below)

Data type mappings
------------------

HDF5 to Igor
~~~~~~~~~~~~~

When copying data from HDF5 to Igor:

+-------------------------------+-------------------+-----------------------------------------------+
| HDF5 Element                  | Igor Equivalent   | Notes                                         |
+===============================+===================+===============================================+
| Group (``/path/group``)       | Data Folder       | Recursive: all subgroups and datasets copied  |
+-------------------------------+-------------------+-----------------------------------------------+
| Dataset (scalar, rank 0)      | 1D Wave [1 pt]    | Numeric or text depending on HDF5 type        |
+-------------------------------+-------------------+-----------------------------------------------+
| Dataset (1D array)            | 1D Wave           | Length matches HDF5 dimension                 |
+-------------------------------+-------------------+-----------------------------------------------+
| Dataset (2D array)            | 2D Wave           | Dimensions match HDF5 shape                   |
+-------------------------------+-------------------+-----------------------------------------------+
| Dataset (≥3D array)           | Not copied (bulk) | Can view preview only                         |
+-------------------------------+-------------------+-----------------------------------------------+
| Attribute (on dataset)        | Wave note         | Encoded as ``key=value;`` pairs               |
+-------------------------------+-------------------+-----------------------------------------------+
| Attribute (on group)          | Sidecar wave      | Stored as ``Igor___folder_attributes``        |
+-------------------------------+-------------------+-----------------------------------------------+
| Enumeration type              | Numeric Wave      | Integer values; enum labels stored as note    |
+-------------------------------+-------------------+-----------------------------------------------+
| Variable-length string        | Text Wave         | (if ``HDF5LoadData`` supports)                |
+-------------------------------+-------------------+-----------------------------------------------+

Igor to HDF5
~~~~~~~~~~~~~

When copying data from Igor to HDF5:

+-------------------------------+---------------------+-----------------------------------------------+
| Igor Element                  | HDF5 Equivalent     | Notes                                         |
+===============================+=====================+===============================================+
| Data Folder                   | Group               | Recursive: sub-folders and waves copied       |
+-------------------------------+---------------------+-----------------------------------------------+
| 1D Wave                       | Dataset (1D)        | Data type matches Igor wave type              |
+-------------------------------+---------------------+-----------------------------------------------+
| 2D Wave                       | Dataset (2D)        | Dimensions preserved                          |
+-------------------------------+---------------------+-----------------------------------------------+
| Numeric Variable              | Dataset (scalar)    | Float64 in HDF5                               |
+-------------------------------+---------------------+-----------------------------------------------+
| String Variable               | Dataset (scalar)    | Fixed-length text in HDF5                     |
+-------------------------------+---------------------+-----------------------------------------------+
| Wave note                     | Attributes          | Parsed as ``key=value;`` pairs                |
+-------------------------------+---------------------+-----------------------------------------------+
| Folder sidecar                | Group attributes    | Contents of ``Igor___folder_attributes``      |
+-------------------------------+---------------------+-----------------------------------------------+

GUI elements and navigation
---------------------------

Panel layout
~~~~~~~~~~~~

The browser window is organized in five main sections:

1. **Title and toolbar** (~45 px)
   - Buttons: "Open Left File...", "Close", "Open Right...", "Close",
     "Igor" (per pane), "Get Help"

2. **File labels** (~20 px)
   - Shows the name and read-only status of the file open in each pane

3. **Tree panes** (~360 px)
   - Two side-by-side ListBox controls, each displaying a collapsible tree
   - Left pane: browse the left-side HDF5 file or Igor experiment
   - Right pane: browse the right-side HDF5 file or Igor experiment

4. **Information and preview section** (~200+ px)
   - **Selected path**: read-only display of the full HDF5 or Igor path
   - **Type / Shape / DType**: inline indicators for dataset rank and datatype
   - **Value preview**: scalar display or ListBox of array rows
   - **Attributes panel**: right-side ListBox showing metadata for the selected
     node

5. **Hints and instructions** (~60 px)
   - Color-coded guidance on tree navigation, context menu, copy behavior,
     and drag-and-drop

Tree navigation
~~~~~~~~~~~~~~~

- **Click [+] or [-]**: toggle expand/collapse of a group.
- **Click on a row name**: select that node and display its value/attributes.
- **Right-click on any row**: open the context menu.

Value preview widget
~~~~~~~~~~~~~~~~~~~~

When you click a node in the tree, the preview section updates:

**Group (folder)**
  - Displays "Group" + count of immediate children
  - Shows list of all attributes (or empty if none)

**Scalar Dataset (numeric or text)**
  - Shows the single value in a read-only text field
  - Example: ``3.14159`` or ``Hello, SAXS!``

**1D Array**
  - Displays the first 50 rows: index (0-based) and value
  - Arrays with >50 points are truncated in the preview

**2D Array**
  - Shows a grid of the first 50 rows × 10 columns
  - Larger arrays are clipped; use an external viewer for full data

**≥3D Arrays**
  - Shape/datatype information is shown, but no value preview is available
  - Use Igor's native HDF5 load commands or Python/HDF5 viewers for full access

**Attributes**
  - All attributes of the selected node are listed in the right-side panel
  - Columns: attribute name and string value
  - Source: native HDF5 attributes, or Igor wave notes (``key=value;`` format),
    or the ``Igor___folder_attributes`` sidecar wave for Igor folders

Copy operations
---------------

You can transfer data in all four directions:

1. **HDF5 → HDF5** (between two open HDF5 files)
2. **HDF5 → Igor** (from an HDF5 file into the current Igor experiment)
3. **Igor → HDF5** (from Igor into an open HDF5 file)
4. **Igor → Igor** (within the current Igor experiment)

Initiating a copy
~~~~~~~~~~~~~~~~~

**Method 1: Right-click context menu**

1. Right-click any node in the tree.
2. Select "*Copy to other pane*" or "*Copy to Igor*" (HDF5 nodes only).

**Method 2: Drag-and-drop**

1. Click and hold on a node.
2. Move the mouse ≥4 pixels (threshold to distinguish drag from click).
3. A ghost TitleBox follows the cursor showing source and destination.
4. Release over a target group/folder in the other pane (or same pane).

Copy semantics
~~~~~~~~~~~~~~

**Name collisions** — existing items at the destination are silently overwritten.
There is no undo.

**Recursive copy** — copying a group or folder copies all child groups,
subgroups, and datasets recursively, preserving the full subtree structure.

**Same-pane drag-and-drop** — this is a **MOVE**: the source is copied to the
destination, then deleted. Moving into a descendant of the source is prevented.

**Cross-pane drag-and-drop** — this is a **COPY**: the source remains unchanged.

**Read-only panes** — copy and modify operations are disabled for read-only
files.

Attribute preservation
----------------------

HDF5 attributes → Igor
~~~~~~~~~~~~~~~~~~~~~~~

When copying from HDF5 to Igor:

- **Dataset attributes** are encoded into the wave note of the destination
  Igor wave as ``key=value;`` pairs.
  Example: ``Q_resolution=0.001; units=1/Angstrom; sample_name=SiO2 powder;``

- **Group attributes** are stored in a sidecar wave named
  ``Igor___folder_attributes`` inside the destination folder. This matches the
  CanSAS/HDF5gateway convention.

- **Multi-value attributes** (arrays) are encoded as comma-separated lists in
  brackets. Example: ``wavelengths=[0.7, 1.5, 2.3];``

Igor → HDF5 attributes
~~~~~~~~~~~~~~~~~~~~~~~

When copying from Igor to HDF5:

- **Wave notes** are parsed and converted back into HDF5 attributes on the
  dataset. Free-form text (no ``key=`` structure) is stored as a single
  ``IgorWaveNote`` attribute.

- **Folder attributes** (from ``Igor___folder_attributes``) are converted to
  HDF5 group attributes.

- **Text values** are preserved as HDF5 string attributes.
- **Numeric values** are preserved with their original precision.

Drag-and-drop mechanics
-----------------------

A drag is recognized only when the mouse moves ≥4 pixels from the initial click
position. While dragging:

- A **ghost TitleBox** follows the cursor showing the source node and target.
- The target tree has a **focus ring** to indicate the drop destination.

You can drop onto any group (HDF5) or folder (Igor), including the root group
(``/``). You cannot drop into your own descendant or into a read-only pane.

After a successful drop:
- Both trees refresh to show the updated structure.
- If the operation was a MOVE (same pane), the source item is removed.
- If it was a COPY (cross-pane), the source remains.

Right-click context menu
------------------------

Right-click any node in either tree to open the context menu.

**Copy to Other Pane** — copies the node (and all children if a group/folder)
to the opposite pane. Destination is the currently selected group in the other
pane, or the root if nothing is selected.

**Copy to Igor Experiment** (HDF5 nodes only) — copies the HDF5 node into the
Igor experiment root, creating a wave or folder with the same name.

**Create Group/Folder** (on a group/folder) — prompts for a name and creates an
empty group/folder at that location.

**Delete** — deletes the selected node and all its children.

.. warning::

   Deletion from an HDF5 file is written to disk immediately and is
   irreversible. Back up important data before deleting.

**Show Metadata** (datasets only) — shows the full HDF5 path, datatype, shape,
attributes, and compression.

**Plot 1D Wave** (1D numeric datasets only) — creates an Igor graph of the wave
as intensity vs. index.

Supported data types
--------------------

Numeric types
~~~~~~~~~~~~~

- **int8, int16, int32, int64** — signed integers
- **uint8, uint16, uint32, uint64** — unsigned integers (stored as signed in
  Igor; values >2\ :sup:`31`−1 overflow)
- **float32, float64** — single and double precision floats
- **Complex64, Complex128** — stored as 2-column waves in Igor

Text types
~~~~~~~~~~

- **Fixed-length strings** — stored as text waves
- **ASCII and UTF-8** — both supported

Unsupported or limited types
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- **Compound types** — not supported; use raw HDF5 access
- **Variable-length strings** — limited support
- **Opaque types** — not supported
- **≥3D arrays** — metadata viewable but no bulk load or preview
- **References** (HDF5 region/object) — not dereferenced automatically

Limitations and troubleshooting
--------------------------------

File access issues
~~~~~~~~~~~~~~~~~~

**"file opened READ-ONLY"** — the file is on a read-only volume or lacks write
permissions. Read and copy operations still work; write operations are disabled.
To fix: copy the file to a writable location or change permissions.

**"could not open file" or FileID = −1** — the file is locked by another
process (HDF5View, Python, another Igor instance). Close the file elsewhere and
try again.

**Stale FileID errors after restart** — if Igor crashes with an HDF5 file open,
stale references may appear on the next launch. Kill and reopen the browser
panel; ``IR3HB_InitPackage()`` resets all file IDs to −1.

Data type conversion issues
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Unsigned integer overflow** — Igor Pro waves use signed integers only. Unsigned
values >2\ :sup:`31`−1 will overflow in signed representation. For high-bit-
depth unsigned data, use Python or raw HDF5 tools.

**Text vs. numeric ambiguity** — datasets containing only numbers but stored as
strings (e.g., ``"123"``) are imported as text waves to preserve fidelity.

Large arrays
~~~~~~~~~~~~

Igor allocates the entire array in RAM. If an HDF5 dataset exceeds available
memory, loading will fail. Solutions: increase system RAM, load a data slab via
Python, or use an external HDF5 viewer.

Previews are limited to 50 rows (and 10 columns for 2D). To inspect the full
data, copy the wave to Igor and use the Data Browser, or use an external tool.

Attribute round-tripping
~~~~~~~~~~~~~~~~~~~~~~~~

Wave notes (containing encoded attributes) travel with an Igor wave when it is
copied. However, if the destination wave already exists and is overwritten, the
old note is lost.

The sidecar wave ``Igor___folder_attributes`` uses a special naming convention
to avoid collisions. Avoid naming your Igor folder items starting with
``Igor___``.

Example workflows
-----------------

Workflow 1: Import calibration data from a beamline HDF5 file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open the browser: **Macros → HDF5 Browser** (or call
   ``IR3HB_HDF5Browser()``).
2. Click "*Open Left File...*" and select the HDF5 file.
3. Expand the tree to find the calibration dataset.
4. Right-click and select "*Copy to Igor Experiment*".
5. The wave appears in your Igor root data folder. Inspect the preview and
   attributes; rename or move as needed.

Workflow 2: Compare two datasets from different HDF5 files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open the browser.
2. Click "*Open Left File...*" and select the first HDF5 file.
3. Click "*Open Right...*" and select the second HDF5 file.
4. Navigate to the datasets in each pane and click to preview them side by side.
5. Inspect Type/Shape/DType and attributes. Drag cross-pane to copy if needed.

Workflow 3: Reorganize an HDF5 file structure
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open the browser and open an HDF5 file with write access.
2. Right-click a parent group and select "*Create Group*" to add new groups.
3. Drag datasets to their new locations within the same pane (same-pane = MOVE).
4. Right-click and select "*Delete*" to remove misplaced items.
5. Changes are written to disk automatically.

Workflow 4: Export an Igor experiment folder to HDF5
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open the browser. Open a target HDF5 file on the right; click "*Igor*" on
   the left.
2. In the left pane, find the Igor folder to export.
3. Right-click and select "*Copy to Other Pane*".
4. Select the destination group in the right pane.
5. The entire folder with all waves, sub-folders, and attributes is recursively
   copied into the HDF5 file.

Technical details
-----------------

Browser state storage
~~~~~~~~~~~~~~~~~~~~~

All browser state is stored under:

.. code-block:: igor

   root:Packages:Irena:HDF5Browser:Left/Right:
      FilePath, FileName           (SVAR: file path and name)
      FileID                       (NVAR: HDF5 file ID, -1 if closed)
      IsReadOnly                   (NVAR: 1 if read-only, 0 if read-write)
      SourceType                   (SVAR: "HDF5" or "Igor" or "" if closed)
      SelectedFullTreeIdx          (NVAR: index into FullTreeText of selected node)
      SelectedPath                 (SVAR: full HDF5 or Igor path of selected node)
      SelectedKind                 (SVAR: "G" for group/folder, "D" for dataset/wave)
      SelectedDType                (SVAR: HDF5 datatype class or Igor wave type)
      SelectedShape                (SVAR: shape string, e.g., "50 x 10" or "scalar")

      FullTreeText                 (text wave, 3 cols: path, name, kind)
      FullTreeMeta                 (numeric wave, 3 cols: depth, parentRow, hasChildren)
      ExpandedState                (numeric 0/1 wave: expanded or collapsed per node)
      VisibleRows                  (numeric wave: indices into FullTree of visible rows)
      TreeListWave                 (text wave, 2 cols: marker + node name)
      TreeSelWave                  (numeric wave: ListBox selection bitmap)

      ScalarValueStr               (SVAR: display string for scalar value preview)
      PreviewListWave              (text wave: preview rows for 1D/2D array display)
      AttrListWave                 (text wave, 2 cols: attr name, attr value)

Tree data structure
~~~~~~~~~~~~~~~~~~~

- **FullTreeText**: one row per node; columns: full path, short name, kind
  (``"G"`` or ``"D"``)
- **FullTreeMeta**: parallel numeric wave; columns: depth, parentRow index,
  hasChildren flag
- **ExpandedState**: 0/1 parallel to FullTree (0 = collapsed, 1 = expanded)
- **VisibleRows**: indices into FullTree for currently visible rows (rebuilt on
  expand/collapse)
- **TreeListWave**: text fed to the ListBox (``+``/``-`` marker + indented name)

Getting help and reporting bugs
--------------------------------

- **In Igor**: click the red "*Get Help*" button in the browser window.
- **Documentation**: this page serves as the primary reference.
- **Bug reports or feature requests**: contact the Irena development team or
  file an issue on the project repository.

Version history
---------------

**v1.04 (2026-05-13)**
  Drag-and-drop between tree panes and within the same pane.
  Cross-pane drag = COPY; same-pane drag = MOVE. Movement threshold 4 px.
  Drop into own descendant is prevented. Ghost TitleBox visual feedback.

**v1.03 (2026-05-12)**
  HDF5 attribute preservation across all copy directions.
  Dataset attributes encoded as wave notes (``key=value;`` format).
  Group attributes stored as sidecar wave ``Igor___folder_attributes``
  (CanSAS-compatible). Multi-value attributes as ``[v1,v2,v3]`` in notes.

**v1.02 (2026-05-12)**
  Copy buttons and right-click context menu.
  All four copy directions; recursive group/folder copy; silent overwrite.
  Context menu: copy, create group, delete, show metadata, plot 1D.

**v1.01 (2026-05-12)**
  Igor experiment as a data source (per-pane toggle).
  Walks Igor data folders, waves, variables, and strings.

**v1.00 (2026-05-12)**
  Initial release: two-pane HDF5 browser with collapsible tree navigation.
  Value preview widget; attribute listing.

See also
--------

- Igor Pro HDF5 documentation: https://docs.wavemetrics.com/igorpro/HDF5/
- HDF5 standard: https://www.hdfgroup.org/
- CanSAS HDF5 gateway (attribute conventions):
  https://www.sas.org/fileadmin/user_upload/cansashdf5/cansashdf5.html
