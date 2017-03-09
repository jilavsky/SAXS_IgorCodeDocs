Other Tools
===========

QRS data folder creation tool
-----------------------------

Introduction
~~~~~~~~~~~~

Many users may have QRS named data in unstructured way – that is all
data placed in one folder, very often “root” folder. This is not very
convenient place for the data, since the Irena macros make heavy use of
the folder structure. To help the users, I made little simple tool,
which should in most cases create successfully folder structure for this
type of data.

Start the tool from “SAS” “Create QRS folder structure”.

.. figure:: media/OtherTools1.png
   :align: left
   :height: 580px

Note that all my imported data are in “root:SAS:ImportedData:” folder.
They can be in any folder in the Igor experiment. Note few controls in
the panel just created.

“\ *Select folder with data”* this popup will list ONLY folders
containing triplets of QRS named data. Select folder, which contains
data you want to convert. In case of this example the
“root:SAS:ImportedData:” folder

*“Where to create new data folder?”* Input full folder name to folder,
in which you want to create new folders with the separated data

*“Backup old data to”* input full folder name where you want to put
backup copy of old data. If empty, backup will not be created.

.. figure:: media/OtherTools2.png
   :align: left
   :height: 580px


Select appropriate folder with data and push button “Convert structure”.
Result can be seen above – folder “Imported data” is now empty and new
folders which are named by sample names (using the name from QRS naming
structure) were created. Each contains QRS named triplet of waves.

Note, that copy of original data is now in root:SAS\_Data\_Backup
folder:

.. figure:: media/OtherTools3.png
   :align: left
   :height: 580px


Few comments.

This tool is relatively simple and does not do much checking. It will
not be able to remove waves, which are part of any graph (or for other
reasons Igor refuses to remove them). It will create new copies of these
data, it just cannot remove the waves in use.

The folders with data are never overwritten, if folder of the particular
name exists, index starting from 0 will be attached to the name.

Do not backup into the same place where the data are coming from. Make
separate backup into separate folder.

Data, for which the code does not find properly named QRS triplet of
waves are not touched.

There is no checking for wave length or other validity, all what is used
is the names of the waves.

The code does not know about any “name extensions”, so data named
“R\_myName\_BkgSub” are treated as separate data from original data
“R\_myName”…

I assume, that your names are legal and valid. The code may fail on
liberal names (names with spaces and other weird characters). I need to
test that later. This should not be a problem, since most users with the
data needing this treatment should have standard (non-liberal) names, or
the code used to create these should not work..
