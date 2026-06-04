.. _irena-other-tools:
.. _other_tools:

Other Tools
===========

.. index::
    Irena; Create QRS structure

QRS data folder creation tool
------------------------------

Many users may have QRS-named data in an unstructured arrangement — for example,
all datasets placed directly in the ``root`` folder. Because Irena makes heavy
use of folder structure, a simple tool is provided to create the standard
folder hierarchy from such data.

Start the tool from SAS → "*Create QRS folder structure*".

.. Figure:: media/OtherTools1.png
   :align: left
   :height: 580px

"*Select folder with data*" — Lists only folders containing QRS-named triplets.
Select the folder containing the data to reorganize (e.g., ``root:SAS:ImportedData:``).

"*Where to create new data folder?*" — Enter the full path to the folder where
new per-sample subfolders will be created.

"*Backup old data to*" — Enter the full path for a backup copy of the original
data. Leave empty to skip backup.

.. Figure:: media/OtherTools2.png
   :align: left
   :height: 580px

Select the appropriate folder and click "*Convert structure*". The result is shown
above — the source folder is empty and new folders named after each sample (using
the QRS name structure) have been created, each containing a QRS wave triplet.

.. Figure:: media/OtherTools3.png
   :align: left
   :height: 580px

A backup copy is placed in ``root:SAS_Data_Backup``.

Notes on this tool:

* The tool does not extensively validate input. Waves that are part of an open
  graph or that Igor refuses to remove for other reasons will not be deleted;
  copies will still be created.
* Existing folders of the same name are not overwritten — an index starting from
  0 is appended to make the name unique.
* Do not specify the backup folder inside the source data folder.
* QRS triplets that are not properly named are not touched.
* Wave lengths and other metadata are not validated — only wave names are used.
* Name extensions are not recognized: ``R_myName_BkgSub`` is treated as a
  distinct dataset from ``R_myName``.
* Names with spaces or other unusual characters may cause errors; standard
  names should work reliably.

.. _ResultsNotebook:

Results Notebook
-----------------

Various tools include a "*Save results (notebook)*", "*Paste to Notebook*", or
similar button. These buttons save data to an internal notebook created by
*Irena* called *ResultsNotebook*. The content depends on the tool — typically a
graph, the data source, and a results summary.

The notebook can be closed at any time; it is hidden, not deleted. Reopen it
from the Irena menu: SAS → Support tools → *Show Results Notebook*. The notebook
can be saved as an RTF file for editing in any word processor.

Logging feature
----------------

.. index::
    Irena; Logging feature

This feature is under development and currently works only for standard models.
Future releases will make these records more complete and useful.

The logbook can be viewed by selecting "*Show SAS logbook*" (second item in the
SAS menu). Below is an example of the current logbook format::

    This is log results of SAS fitting with modeling macros Irena.

    1/5/02, 5:47 PM

    ***********************************************

    Parameters before starting Fitting on the data from:
    root:USAXS:'S5_Al2O3 1um':

    Number of modelled distributions: 1

    SAS background = 0.15, was fitted? = 0 (yes=1/no=0)

    *********** Distribution 1

    Particle shape: sphere
    Distribution type: LogNormal
    Contrast 120
    Volume 0.09 , fitted? = 0
    Location 250 , fitted? = 1
    Scale 300.1 , fitted? = 1
    Shape 0.5 , fitted? = 0
    Mean 575.21
    Median 550.12
    Mode 483.83
    FWHM 291.36

    ***********************************************

    Results of the Fitting on the data from: root:USAXS:'S5_Al2O3 1um':
    ...

Final comments
==============

This manual is a living document and is updated as tools evolve. If you find
missing content or errors, please contact the developer.
