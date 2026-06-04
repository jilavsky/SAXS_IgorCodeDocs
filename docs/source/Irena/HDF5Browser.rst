.. _irena-hdf5-browser:

HDF5 Browser
============

.. contents::
   :local:

Overview
========

The HDF5 Browser is a custom Igor Pro tool for exploring, browsing, and transferring data between HDF5 files and Igor Pro experiments. It provides a Data Browser-style two-pane interface where each pane independently displays either an HDF5 file or the current Igor experiment's data folder hierarchy.

**Key Features:**

- **Two independent panes**: simultaneously browse two HDF5 files, or one HDF5 file and the Igor experiment
- **Collapsible tree navigation**: expand/collapse groups to navigate hierarchies
- **Value preview widget**: inline display of scalar values, text strings, and previews of 1D/2D arrays
- **Attribute display**: view metadata (HDF5 attributes, Igor wave notes, folder attributes)
- **Copy operations**: transfer datasets/groups between panes and into/out of Igor Pro
- **Drag-and-drop**: intuitively move or copy items using mouse drag
- **Right-click context menu**: quick access to copy, inspect, plot, and folder management functions
- **Attribute preservation**: round-trip HDF5 attributes through Igor experiment exports/imports via wave notes and sidecar waves


File Types and Data Sources
=============================

Supported HDF5 File Formats
---------------------------

The browser accepts standard HDF5 files and Igor experiment files (which can embed HDF5):

- **.h5** — Standard HDF5 file format
- **.h5xp** — Igor Pro experiment file containing HDF5 data (read/write)
- **.pxp** — Igor Pro experiment file (readable as HDF5 via Igor's native HDF5 interface)

Igor Experiment as Data Source
-------------------------------

Each pane can also browse the **current Igor experiment** directly by clicking the "Igor" button. This displays:

- **Folders** — mapped from Igor data folders
- **Waves** — individual Igor waves (numeric or text)
- **Variables** — scalar numeric values
- **Strings** — text variables
- **Attributes** — wave notes and folder sidecar attributes (see `Attribute Preservation`_ below)


Data Type Mappings
===================

HDF5 to Igor
------------

When copying data from HDF5 to Igor:

+-------------------------------+-------------------+-----------------------------------------------+
| HDF5 Element                  | Igor Equivalent   | Notes                                         |
+===============================+===================+===============================================+
| Group (``/path/group``)       | Data Folder       | Recursive: all subgroups and datasets copied |
+-------------------------------+-------------------+-----------------------------------------------+
| Dataset (scalar, rank 0)      | 1D Wave [1 pt]    | Numeric or text depending on HDF5 type      |
+-------------------------------+-------------------+-----------------------------------------------+
| Dataset (1D array)            | 1D Wave           | Length matches HDF5 dimension                |
+-------------------------------+-------------------+-----------------------------------------------+
| Dataset (2D array)            | 2D Wave           | Dimensions match HDF5 shape                  |
+-------------------------------+-------------------+-----------------------------------------------+
| Dataset (≥3D array)           | Not copied (bulk) | Can view preview only; contact support       |
+-------------------------------+-------------------+-----------------------------------------------+
| Attribute (on dataset)        | Wave note         | Encoded as ``key=value;`` pairs             |
+-------------------------------+-------------------+-----------------------------------------------+
| Attribute (on group)          | Sidecar wave      | Stored as ``Igor___folder_attributes``       |
+-------------------------------+-------------------+-----------------------------------------------+
| Enumeration type              | Numeric Wave      | Integer values; enum labels stored as note   |
+-------------------------------+-------------------+-----------------------------------------------+
| Variable-length string        | Text Wave         | (if ``HDF5LoadData`` supports; otherwise TBD)|
+-------------------------------+-------------------+-----------------------------------------------+

Igor to HDF5
------------

When copying data from Igor to HDF5:

+-------------------------------+---------------------+-----------------------------------------------+
| Igor Element                  | HDF5 Equivalent     | Notes                                         |
+===============================+=====================+===============================================+
| Data Folder                   | Group               | Recursive: sub-folders and waves copied      |
+-------------------------------+---------------------+-----------------------------------------------+
| 1D Wave                       | Dataset (1D)        | Data type matches Igor wave type              |
+-------------------------------+---------------------+-----------------------------------------------+
| 2D Wave                       | Dataset (2D)        | Dimensions preserved                         |
+-------------------------------+---------------------+-----------------------------------------------+
| Numeric Variable              | Dataset (scalar)    | Float64 in HDF5                               |
+-------------------------------+---------------------+-----------------------------------------------+
| String Variable               | Dataset (scalar)    | Fixed-length text in HDF5                     |
+-------------------------------+---------------------+-----------------------------------------------+
| Wave note                     | Attributes         | Parsed as ``key=value;`` pairs               |
+-------------------------------+---------------------+-----------------------------------------------+
| Folder sidecar                | Group attributes    | Contents of ``Igor___folder_attributes``     |
+-------------------------------+---------------------+-----------------------------------------------+

GUI Elements and Navigation
============================

Panel Layout
------------

The browser window is organized in five main sections:

1. **Title and toolbar** (~45 px)
   - Window title and decorative line
   - Buttons: "Open Left File...", "Close", "Open Right...", "Close", "Igor" (per pane), "Get Help"

2. **File labels** (~20 px)
   - Shows the name and read-only status of the file open in each pane

3. **Tree panes** (~360 px)
   - Two side-by-side ListBox controls, each displaying a collapsible tree
   - Left pane: browse left-side HDF5 file or Igor experiment
   - Right pane: browse right-side HDF5 file or Igor experiment

4. **Information and preview section** (~200+ px)
   - **Selected path**: read-only display of the full HDF5 or Igor path of the selected node
   - **Type / Shape / DType**: inline indicators for dataset rank, dimensions, and HDF5 datatype
   - **Value preview**: scalar display (SetVariable) or ListBox of array rows
   - **Attributes panel**: right-side ListBox showing all metadata for the selected node

5. **Hints and instructions** (~60 px)
   - Color-coded guidance on tree navigation, right-click menu, copy behavior, and drag-and-drop


Tree Navigation
---------------

- **Click [+] or [-]**: toggle expand/collapse of a group. Child items appear/disappear indented below.
- **Click on a row name**: select that node and display its value/attributes in the preview section below.
- **Right-click on any row**: open context menu for operations (see `Right-Click Context Menu`_).


Value Preview Widget
---------------------

When you click a node in the tree, the preview section below updates:

**Group (folder)**
  - Displays "Group" + count of immediate children
  - Shows list of all attributes (or empty if none)
  - No value display (groups are containers, not data)

**Scalar Dataset (numeric or text)**
  - Shows the single value in a read-only text field
  - Type shown as "scalar / numeric" or "scalar / text"
  - Example: ``3.14159`` or ``Hello, SAXS!``

**1D Array**
  - Displays the first 50 rows in a two-column ListBox: index (0-based) and value
  - If the array has >50 points, the display is truncated; rest is not shown
  - Type shown as "1D array, N points, dtype"
  - Useful for inspecting intensity curves or 1D results

**2D Array**
  - Shows a grid (ListBox with multiple columns) of the first 50 rows × 10 columns
  - If larger, the preview is clipped; see HDF5 file directly for full data
  - Type shown as "2D array, M × N, dtype"
  - Common use: viewing Q-I plots, detector images, or calibration tables

**≥3D Arrays**
  - Preview is not supported
  - You will see the shape/datatype information, but no graphical preview
  - If you need to work with 3D+ arrays, use Igor's native HDF5 load commands or external Python/HDF5 viewers

**Attributes**
  - All attributes of the selected node (whether it is a group or dataset) are listed in the right-side panel
  - Columns: attribute name (left) and string value (right)
  - For HDF5: native HDF5 attributes from the file
  - For Igor waves: parsed from the wave note (``key=value;`` format)
  - For Igor folders: parsed from the sidecar wave ``Igor___folder_attributes``


Copy Operations
================

Overview
--------

You can transfer data in all four directions:

1. **HDF5 → HDF5** (between two open HDF5 files in left and right panes)
2. **HDF5 → Igor** (from HDF5 file into current Igor experiment)
3. **Igor → HDF5** (from Igor experiment into an open HDF5 file)
4. **Igor → Igor** (within the current Igor experiment; essentially a local copy/move)

Initiating a Copy
-----------------

**Method 1: Right-click context menu**
  1. Right-click any node in the tree
  2. Select from the menu:
     - "Copy to other pane" — copies to the opposite pane
     - "Copy to Igor" (only if right-clicking HDF5 node) — copies to current Igor experiment
     - (More options may be available; see `Right-Click Context Menu`_ below)

**Method 2: Drag-and-drop**
  1. Click and hold on a node
  2. Move the mouse ≥4 pixels (the movement threshold to distinguish drag from a simple click)
  3. Visual ghost feedback (a TitleBox) follows the mouse to show drop destination
  4. Release over the target group/folder in the opposite pane (or same pane)
  5. Drop confirms; copy or move is executed

Copy Semantics
--------------

**Name collisions**
  - If the destination already contains an item with the same name, the existing item is **silently overwritten** (no dialog).
  - Plan your copy operations accordingly; there is no undo.

**Recursive copy**
  - If you copy a **group or folder**, all child groups, subgroups, and datasets are copied recursively.
  - This preserves the entire subtree structure at the destination.

**Same-pane operations (drag-and-drop within same pane)**
  - Dragging within the same pane is a **MOVE** operation: the source is copied to the destination, then the source is deleted.
  - Useful for reorganizing Igor experiments or HDF5 file structures.
  - **Caveat**: Moving into a descendant of the source is prevented to avoid corruption.

**Cross-pane operations (drag-and-drop between panes)**
  - Dragging from one pane to another is a **COPY** operation: the source remains unchanged.
  - Use this to duplicate data or transfer between HDF5 files.

**Read-only panes**
  - If an HDF5 file is open in read-only mode (e.g., on a read-only file system), the "Copy to X" options will be disabled for that pane.


Attribute Preservation
======================

Overview
--------

One of the most important features is that attributes (metadata) are preserved when copying data between HDF5 and Igor Pro. This ensures that calibration information, sample metadata, and other scientific information travels with the data.

HDF5 Attributes → Igor
----------------------

When copying from HDF5 to Igor:

- **Dataset attributes** are encoded into the **wave note** of the destination Igor wave as ``key=value;`` pairs, one per semicolon-separated item.
  - Example: ``Q_resolution=0.001; units=1/Angstrom; sample_name=SiO2 powder;``

- **Group attributes** are stored in a sidecar wave named ``Igor___folder_attributes`` inside the destination folder.
  - This wave is automatically created and contains the encoded attribute list.
  - This pattern matches the CanSAS/HDF5gateway convention.

- **Multi-value attributes** (arrays in HDF5) are encoded as comma-separated lists enclosed in brackets.
  - Example: ``wavelengths=[0.7, 1.5, 2.3];``

Igor → HDF5 Attributes
----------------------

When copying from Igor to HDF5:

- **Wave notes** are parsed and converted back into HDF5 attributes on the dataset.
  - The browser looks for ``key=value;`` pairs in the note and creates corresponding HDF5 attributes.
  - If the wave note contains free-form text (no ``key=`` structure), it is stored as a single HDF5 attribute named ``IgorWaveNote``.

- **Folder attributes** (from the ``Igor___folder_attributes`` sidecar wave) are converted to HDF5 group attributes.
  - These are automatically attached to the HDF5 group corresponding to the Igor folder.

- **Text values** are preserved as HDF5 string attributes.
- **Numeric values** are preserved with their original precision (float64 for doubles, etc.).


Drag-and-Drop Mechanics
=======================

Movement Threshold
------------------

A drag is only recognized if the mouse moves **≥4 pixels** from the initial click position. This threshold prevents accidental drags when you are simply clicking to select a node.

Visual Feedback
---------------

While dragging:
- A **TitleBox ghost** follows your mouse cursor, showing the source node and destination group.
- The destination tree has a **focus ring** drawn around it to indicate where the drop will occur.
- This visual feedback is removed when you release the mouse.

Drop Behavior
-------------

You can drop on:
- Any **group** (HDF5) or **folder** (Igor) to move/copy the source into it
- The **root group** (``/``) to place the item at the top level

You **cannot** drop:
- Into your own descendant (prevents circular references and corruption)
- Into a read-only pane (disabled automatically)

After a successful drop:
- The trees refresh to show the new structure
- The source selection is cleared (to avoid stale references)
- If it was a MOVE (same pane), the source item is gone
- If it was a COPY (cross-pane), the source remains


Right-Click Context Menu
=========================

Right-click any node in either tree to open the context menu:

**Copy to Other Pane**
  - Copies the selected node (and all children if it is a group/folder) to the opposite pane.
  - Destination is the currently selected group/folder in the other pane.
  - If nothing is selected, copy goes to the root group.
  - Cross-pane copy preserves all attributes.

**Copy to Igor Experiment** (HDF5 nodes only)
  - Copies the HDF5 node into the current Igor experiment's root data folder.
  - Creates a wave (or folder if it is a group) with the same name as the HDF5 object.
  - Useful for exporting individual datasets or entire group hierarchies.

**Create Group/Folder** (on a group/folder only)
  - Opens a dialog asking for a new group/folder name.
  - Creates an empty group/folder at that location.
  - Useful for organizing data before copying items into the new folder.

**Delete**
  - Deletes the selected node (and all children if it is a group/folder).
  - **This is irreversible** if the source is an HDF5 file (will be written to disk).
  - Use with caution; consider a copy/backup first if unsure.

**Show Metadata** (datasets only)
  - Opens a dialog showing detailed information about the dataset:
    - Full HDF5 path
    - Datatype and element byte size
    - Shape (rank and dimensions)
    - Attributes
    - Compression (if any)
  - Useful for understanding the structure of datasets before deciding how to process them.

**Plot 1D Wave**
  - Appears only for 1D numeric datasets.
  - Creates an Igor graph showing the 1D wave as intensity vs. index (or Q, if the dataset has a calibration attribute).
  - Useful for quick visual inspection of 1D data (intensity curves, calibration tables, etc.).
  - (In the future: may support axis labels from metadata.)


Supported Data Types
====================

Numeric Types
-------------

- **int8, int16, int32, int64** — signed integers
- **uint8, uint16, uint32, uint64** — unsigned integers (converted to signed storage if necessary)
- **float32, float64** — single and double precision floats
- **Complex64, Complex128** — complex numbers (stored as 2-column wave in Igor if loaded)

Text Types
----------

- **Fixed-length strings** — stored as text waves
- **ASCII** and **UTF-8** — both supported

Unsupported / Limited Types
-----------------------------

- **Compound types** (nested struct-like records in HDF5) — preview and bulk load is not supported; requires custom handling via raw HDF5 access
- **Variable-length strings** — may work in limited cases; contact support
- **Opaque types** — not supported
- **≥3D arrays** — can be browsed and metadata viewed, but no preview or bulk load in the browser (use Python / raw HDF5 tools)
- **References** (HDF5 region/object references) — not dereferenced automatically


Limitations and Troubleshooting
================================

File Access Issues
------------------

**"file opened READ-ONLY"**
  - The HDF5 file is on a read-only volume or you lack write permissions.
  - You can still read and copy from the file, but cannot create, modify, or delete within it.
  - To fix: save the file to a writable location or change file permissions.

**"could not open file" or FileID=-1**
  - File may be locked by another process (HDF5View, Python, another Igor instance).
  - Solution: close the file in the other application and try again.

**Stale FileID errors on restart**
  - If Igor crashes with an HDF5 file open, the next restart may show stale file references.
  - Solution: kill and reopen the browser panel. The `IR3HB_InitPackage()` function will reset all file IDs to -1.

Data Type Conversions
---------------------

**Losing precision with uint8..uint64 to signed Igor storage**
  - Igor Pro 10 waves are signed integers only.
  - Unsigned integer data is copied as-is, but values > 2^31-1 will overflow in signed representation.
  - Solution: if you have high bit-depth unsigned data, use Python or raw HDF5 tools instead.

**Text vs. numeric ambiguity**
  - Datasets containing only numbers (e.g., a string ``"123"``) are stored as text waves in Igor, not numbers.
  - This preserves fidelity, but means you must be aware of the actual type when processing.

Large Arrays
------------

**Bulk load of very large arrays**
  - Igor Pro allocates the entire array in RAM. If an HDF5 dataset is larger than available memory, loading will fail.
  - Solution: either increase system RAM, load only a slab of the data via Python/raw HDF5, or contact support.

**Preview truncation**
  - Previews are limited to 50 rows (and 10 columns for 2D).
  - To see the full data, load the wave via "Copy to Igor" and inspect in Igor's Data Browser, or use external viewers.

Attribute Round-Tripping
------------------------

**Wave notes are cleared on copy to new wave**
  - If you copy an Igor wave to another location, the wave note (containing attributes) travels with it.
  - However, if the destination wave already exists and you overwrite it, the old note is lost.
  - Plan accordingly; keep backups of important metadata.

**Sidecar waves may interfere with data folders**
  - The sidecar wave ``Igor___folder_attributes`` uses a special naming convention to avoid collisions.
  - If you have a data folder item with a name starting with ``Igor___``, it may interact unexpectedly.
  - Recommendation: avoid that naming pattern in your Igor data folders.


Example Workflows
=================

Workflow 1: Import calibration data from an HDF5 beamline file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open the HDF5 browser: **Macros → HDF5 Browser** (or call ``IR3HB_HDF5Browser()`` from the command line).
2. Click "Open Left File..." and select your beamline's HDF5 file (e.g., ``scan_12345.h5``).
3. In the left pane, expand the tree to find the calibration dataset (e.g., ``/calibration/energy_keV`` or ``/instrument/detector/efficiency``).
4. Select the calibration dataset.
5. Right-click and choose "Copy to Igor Experiment".
6. The wave appears in your Igor root data folder.
7. Inspect the value preview and attributes.
8. Move or rename the wave as needed.

Workflow 2: Compare two datasets from different HDF5 files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open the HDF5 browser.
2. Click "Open Left File..." and select the first HDF5 file.
3. Click "Open Right..." and select the second HDF5 file.
4. In each pane, navigate to the datasets you want to compare.
5. Click on the dataset in the left pane to preview it.
6. Click on the corresponding dataset in the right pane to preview it side-by-side.
7. Inspect the Type/Shape/DType and attributes to understand the structure.
8. If they are identical, you can drag from one to the other to copy into the other file (cross-pane copy).

Workflow 3: Reorganize an HDF5 file structure
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open the HDF5 browser and open an HDF5 file for editing (must have write access).
2. Create new groups/folders as needed by right-clicking a parent group and selecting "Create Group".
3. Drag datasets from their current location to the new groups (same-pane drag = MOVE).
4. Delete misplaced items by right-clicking and selecting "Delete".
5. When done, the HDF5 file is automatically updated on disk.

Workflow 4: Export an Igor experiment folder to HDF5
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open the HDF5 browser.
2. Open an existing HDF5 file (or a new empty one) on the right side.
3. Click "Igor" on the left side to browse the Igor experiment.
4. In the left pane, find the folder you want to export (e.g., ``root:SAS:MySample:``).
5. Right-click the folder and select "Copy to Other Pane".
6. Select where in the HDF5 file you want the folder to go (e.g., click on the ``/samples`` group in the right pane, then right-click and "Copy to Other Pane").
7. The entire folder and all waves/subfolders are recursively copied into the HDF5 file.
8. Attributes (from wave notes and sidecar waves) are preserved as HDF5 attributes.


Technical Details
=================

Data Folder Organization
------------------------

All browser state is stored under:

.. code-block:: igor

   root:Packages:Irena:HDF5Browser:Left/Right:
      FilePath, FileName           (SVAR: file path and name)
      FileID                       (NVAR: HDF5 file ID, -1 if closed)
      IsReadOnly                   (NVAR: 1 if read-only, 0 if read-write)
      SourceType                   (SVAR: "HDF5" or "Igor" or "" if closed)
      SelectedFullTreeIdx          (NVAR: index into FullTreeText of selected node)
      SelectedPath                 (SVAR: full HDF5 or Igor path of selected node)
      SelectedKind                 (SVAR: "G" for group/folder, "D" for dataset/wave, "" if closed)
      SelectedDType                (SVAR: HDF5 datatype class or Igor wave type)
      SelectedShape                (SVAR: shape string, e.g., "50 x 10" or "scalar")

      FullTreeText                 (text wave, 3 cols: path, name, kind)
      FullTreeMeta                 (numeric wave, 3 cols: depth, parentRow, hasChildren)
      ExpandedState                (numeric 0/1 wave: expanded or collapsed state per node)
      VisibleRows                  (numeric wave: indices into FullTree of visible rows)
      TreeListWave                 (text wave, 2 cols: ["+/-", node name] for ListBox)
      TreeSelWave                  (numeric wave: ListBox selection bitmap)

      Preview:                     (data folder holding preview waves)
         (one wave per preview, e.g., ``previewData``)
         (automatically cleared and recreated on each selection)

      ScalarValueStr               (SVAR: display string for scalar value preview)
      PreviewListWave              (text wave: preview rows for 1D/2D array display)
      PreviewSelWave               (numeric wave: ListBox selection bitmap for preview)

      AttrListWave                 (text wave, 2 cols: [attr name, attr value])
      AttrSelWave                  (numeric wave: ListBox selection bitmap for attributes)

Tree Data Structure
-------------------

- **FullTreeText**: holds every group and dataset in the file (one row per node)
  - Column 0: full HDF5 or Igor path (e.g., ``/groups/data/wave1`` or ``root:Folder:subfolder``)
  - Column 1: short name of the node (e.g., ``wave1``)
  - Column 2: kind indicator: ``"G"`` for group/folder, ``"D"`` for dataset/wave

- **FullTreeMeta**: parallel numeric columns for tree bookkeeping
  - Column 0: depth (0 = root, 1 = child of root, etc.)
  - Column 1: parentRow (index into FullTree of the parent node, -1 for root)
  - Column 2: hasChildren (0 or 1; 1 if the node has any children)

- **ExpandedState**: 0/1 wave parallel to FullTree
  - 0 = group is collapsed (children hidden)
  - 1 = group is expanded (children shown)

- **VisibleRows**: numeric wave of indices into FullTree
  - Rebuilt whenever the user expands or collapses a group
  - Contains only the rows that should be currently visible (respects collapsed state)
  - Used to compute row numbers for the ListBox display

- **TreeListWave**: text wave fed to the ListBox
  - Column 0: expand/collapse marker (``"+"`` for collapsed groups, ``"-"`` for expanded groups, blank for leaf nodes) + indentation spaces
  - Column 1: node short name + indentation spaces
  - Rebuilt whenever VisibleRows changes


Getting Help and Reporting Bugs
================================

- **In Igor**: click the red "Get Help" button in the browser window (top right).
- **Documentation**: this file serves as the primary reference.
- **Bug reports or feature requests**: contact the Irena/Indra development team or file an issue on the project repository.


Version History
===============

**v1.04 (2026-05-13)**
  - Phase 5: Drag-and-drop between tree panes and within the same pane.
  - Cross-pane drag = COPY; same-pane drag = MOVE.
  - Movement threshold of 4 px distinguishes drag from click.
  - Drop into own descendant is prevented.
  - Visual ghost TitleBox feedback follows the mouse.

**v1.03 (2026-05-12)**
  - Phase 4: HDF5 attribute preservation across all copy directions.
  - Dataset attributes encoded as wave notes (``key=value;`` format).
  - Group attributes stored as sidecar wave ``Igor___folder_attributes`` (CanSAS-compatible).
  - Recursive walkers for both HDF5↔HDF5 and HDF5↔Igor.
  - Multi-value attributes encoded as ``[v1,v2,v3]`` in notes.

**v1.02 (2026-05-12)**
  - Phase 2: Copy buttons and right-click context menu.
  - Supports all four copy directions: HDF5↔HDF5, HDF5↔Igor, Igor↔Igor.
  - Recursive group/folder copy with silent overwrite on collision.
  - Context menu: copy to other pane, copy to Igor, create group/folder, delete, show metadata, plot 1D.

**v1.01 (2026-05-12)**
  - Added Igor experiment as a data source (per-pane toggle).
  - Each pane can browse HDF5 file or current Igor experiment.
  - Walks Igor data folders, waves, variables, and strings.

**v1.00 (2026-05-12)**
  - Phase 1: Two-pane HDF5 browser with collapsible tree navigation.
  - Value display widget for scalars/strings and 1D/2D array previews.
  - Attribute listing for the selected node.


See Also
========

- Igor Pro HDF5 documentation: https://docs.wavemetrics.com/igorpro/HDF5/
- HDF5 standard specification: https://www.hdfgroup.org/
- CanSAS HDF5 gateway (attribute conventions): https://www.sas.org/fileadmin/user_upload/cansashdf5/cansashdf5.html
