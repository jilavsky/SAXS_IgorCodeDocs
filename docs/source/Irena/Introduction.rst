.. _irena-introduction:
.. _introduction:

.. index::
    Irena; Introduction

Introduction — Irena
====================

Manual |release| for Irena version 2.62 for Igor Pro 9.04 or higher.

|today|

**Jan Ilavsky**

If you use Irena in published work, please cite:

   Jan Ilavsky and Peter R. Jemian, *"Irena: tool suite for modeling and
   analysis of small-angle scattering"*, Journal of Applied Crystallography,
   vol. 42 (2009).

**Acknowledgements — contributors of selected methods:**

  #. Least squares modeling and other methods — Jan Ilavsky
  #. Size distribution — Pete R. Jemian (maximum entropy/regularization)
  #. Unified model — Greg Beaucage
  #. Guinier-Porod model — Bualem Hammouda
  #. Pair distance distribution function — Jan Ilavsky, Pete Jemian (regularization)
  #. Fractals model — Andrew J. Allen
  #. Reflectivity (Parratt's code) — Andrew Nelson
  #. Desmearing — Pete R. Jemian
  #. Ciccariello-Benedetti model — S. Ciccariello

.. note::

   These macros represent a collaborative work in progress and not all features
   may be complete at any given time. While every effort is made to verify
   results, no guarantees can be made as to their reliability. Please verify
   results independently and report any bugs to ilavsky@anl.gov. Support is
   provided on a best-effort basis.

Description
-----------

The *Irena* package is a suite of Igor Pro (WaveMetrics, version 9.04 or
higher) macros for the evaluation of small-angle scattering data. It was
designed to work seamlessly with data from the APS USAXS instrument (currently
beamline 9ID, Advanced Photon Source, Argonne, IL) reduced using the *Indra*
package. It also works with any SAS dataset that provides a scattering vector
(Q), intensity, and optionally intensity uncertainty. Specific releases support
Q-resolution data. The package integrates easily with the *Nika* 2D data
reduction package (QRS naming system) and provides a customizable import tool
for most column-format ASCII data from other SAS instruments.

*Irena* contains the following components:

#. **Simple Fits and Analysis**
    * Guinier
    * Porod
    * Sphere, Spheroid
    * Guinier rod, Guinier sheet
    * Invariant
    * 1D correlation
    * Power law
#. **Size distribution** using maximum entropy, total non-negative least squares (TNNLS), and regularization methods for evaluating small-angle scattering from scatterers represented by various form factors.
#. **Modeling (II)** of SAS from up to 10 model "populations" (size distributions, Unified levels, or diffraction peaks) fit simultaneously to up to 10 datasets. Includes many form factors and structure factors.
#. **Unified Fit** for fitting SAS data using up to 5 levels of combined Guinier and power-law dependencies.
#. **Guinier-Porod model** for fitting SAS data using up to 5 levels.
#. **Pair distance distribution function** (PDDF, p(r)).
#. **Fractal model** — combination of up to 2 mass fractals and 2 surface fractals.
#. **Systems-specific models** including:
    * Debye-Bueche model for scattering from gels
    * Treubner-Strey model for small-angle diffraction
    * Ciccariello-Benedetti model for layers on smooth surfaces
    * Hermans — several versions
#. **BioSAXS tools** — a dedicated toolset supporting the typical BioSAXS workflow:
    * Import data
    * Average, subtract, scale
    * Plot
    * Simple fits
    * Merge SAXS-WAXS
    * Export
    * PDDF + MW (Gnom, etc.)
    * Concentration series
#. **Small-angle diffraction** tool for modeling diffraction in the small-angle range.
#. **Powder diffraction fitting (WAXS)** for fitting peak positions in powder diffraction-type data.
#. **X-ray and neutron reflectivity** calculations using Parratt's recursive method.
#. **Scattering contrast calculator** including anomalous (energy-dependent) effects.
#. **Data import tools** — imports ASCII, HDF5 canSAS NeXus, or canSAS XML files. ASCII data must be in columns separated by whitespace, tabs, or other delimiters. Creates user-friendly logical folder structures within the Igor experiment.
#. **HDF5 Browser** — interactive two-pane tool for browsing, comparing, and transferring data between HDF5 files and Igor Pro experiments. Supports metadata preservation, drag-and-drop, and attribute round-tripping.
#. **Data export tool** — exports to ASCII, HDF5 canSAS NeXus, or canSAS XML files.
#. **Desmearing** for finite-slit-length smeared data.
#. **Data manipulation tools** — merging, smoothing, adding, and subtracting SAS datasets. Input datasets are not required to use the same naming convention.
#. **Two plotting tools** — generate various SAS plot types (Porod, Guinier, Kratky, Zimm, etc.) with basic fitting. Save and reapply plot styles for reproducible publication-ready figures. Plotting tool I supports two types of 3D graphs and movie export.
#. **Data mining tools** — search for results (variables, strings, waves) across Igor experiment folders with flexible output options.
#. **Scripting tool** — automates Size Distribution and Unified Fit analysis across multiple datasets.
#. Folder structure creation tool for unstructured QRS data, and several other utilities.

All methods use as similar an interface as reasonably possible to minimize the learning curve.
