.. _indra-saxs-data-reduction:
.. _reduce_SAXS_data_procedure:
.. _reduce_SAXS_data_panel:

.. index::
    Indra; Reduce SAXS data
    Indra; SAXS data reduction

Reduce SAXS data
================

.. note::

   Reduce USAXS data before reducing SAXS data. The presence of USAXS data
   provides essential information such as instrument slit length, which the
   SAXS reduction step reads automatically.

Collected data arrangement
--------------------------

When you collect data on the USAXS/SAXS/WAXS instrument, data are saved in
folders named using the spec file naming convention: ``MM_DD_userName``. When
USAXS data are collected, a subfolder with the suffix ``_usaxs`` is created;
for SAXS data the suffix is ``_saxs``; for WAXS data it is ``_waxs``.

.. Figure:: media/USAXSComputerDataArrangement.jpg
   :align: center
   :width: 480px

Reduced SAXS data arrangement
------------------------------

After reducing SAXS data, the Igor experiment contains SAXS data in
``root:SAXS:Samplename``, using the ``QRS`` naming
:ref:`system <important.QRS>`. To browse the experiment, use the Data Browser
(Ctrl-B or Cmd-B).

.. Figure:: media/SAXSIgorDataArrangement.jpg
   :align: center
   :width: 480px

.. note::

   Each sample typically produces two processed folders. Folders ending in
   ``_270_30`` contain pinhole-collimated data for merging with desmeared
   (DSM) USAXS data. Folders ending in ``_u`` contain pseudo-slit-smeared SAXS
   data for merging with slit-smeared (SMR) USAXS data.

.. index::
    Indra; SAXS data reduction configuration

SAXS data reduction
-------------------

Data reduction uses the :ref:`Nika package <Introduction_Nika>`. Ensure Nika
is :ref:`installed <Installation>`. Select "*Load Nika 2D SAS macros*" from
the Macros menu, or load "*USAXS, Irena and Nika*" to load all three packages.
This creates the SAS2D menu. From the "*Instrument Configurations*" menu in
SAS2D, select "*APS USAXS-SAXS-WAXS*". This opens the instrument
configuration panel for Nika.

.. Figure:: media/SAXSReductionConfig.jpg
   :align: left
   :width: 500px
   :figwidth: 820px

Select (or keep selected) the "*SAXS*" checkbox and follow the on-screen
instructions in red. Keep other checkboxes at their defaults. The first step
is to click "*Set default settings*", which opens a file dialog. Navigate to
your SAXS data folder and select any data file from your samples (assuming no
geometry changes within the folder). Click Open.

.. Figure:: media/SAXSSelectNXDataFile.jpg
   :align: left
   :width: 500px
   :figwidth: 820px

Nika reads calibration values from the selected file and performs the following
steps automatically:

1. All instrument parameters are read and inserted into the appropriate Nika
   fields.
2. Nika scans for existing USAXS data. If found, it checks whether desmeared
   data (``DSM_Int``, etc.) or slit-smeared data (``SMR_Int``) are present. If
   desmeared data exist, slit smearing is disabled in Nika. If only
   slit-smeared data are found, slit smearing is enabled to generate matching
   SAXS data. The correct slit length is inserted automatically.
3. Nika opens the selected image and displays it.
4. Nika sets the correct calibration checkboxes and inserts the appropriate
   lookup function names for sample thickness, transmission, and normalization.
5. **Mask:** Depending on the "*Mask Less sensitive pixels*" checkbox, Nika
   creates one of two masks. When unchecked (default), a mask covering only
   the detector edges and beamstop bar is applied. When checked, pixels between
   detector chips — which are typically ~1% less sensitive — are also masked.
   See the WAXS data reduction section for an example:
   :ref:`Impact of different mask selection <reduce_WAXS_data_mask>`.
6. **Important:** By default, Nika produces 120 log-Q-spaced bins (reduced
   from ~500 points at maximum resolution). This is appropriate for
   small-angle scattering, but if your data contain diffraction peaks requiring
   high Q resolution, select the "*Max num points?*" checkbox.
7. Nika displays the Blank selection tab, where you select the appropriate
   empty/blank file for your samples.

Select the correct blank file — right-click in the panel and use "*Match Blank*"
if needed. Double-click the file or select it and click "*Load Empty*".

.. Figure:: media/SAXSBlankSelection.jpg
   :align: left
   :width: 500px
   :figwidth: 820px

The blank file is loaded and displayed. Select the appropriate blank for each
group of samples and update it whenever geometry or sample conditions change.

.. Figure:: media/SAXSSampleBlankLoaded.jpg
   :align: left
   :width: 700px
   :figwidth: 820px

Sample and blank are displayed side by side for comparison.

Select the sample file(s) to process and click "*Process images*". Nika
processes all selected files. When USAXS data have been desmeared, Nika
produces pinhole-collimated SAXS data only (folders ending in ``_270_30``).
Folders ending in ``_u`` contain slit-smeared data.

.. Figure:: media/SAXSProcessedDataImg.jpg
   :align: left
   :width: 700px
   :figwidth: 820px

The next steps are to reduce WAXS data (if collected) and merge USAXS and SAXS
data. See :ref:`Switch Nika configurations <switch_nika_configurations>` and
:ref:`Reduce WAXS data <reduce_WAXS_data_procedure>`.
