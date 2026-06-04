.. _irena-pyirena:

pyIrena — Python package
=========================

.. index::
   Irena; pyIrena
   pyIrena; Python
   Python; SAS analysis

`pyIrena <https://github.com/jilavsky/pyirena>`_ is a Python port of the
Irena package developed at Argonne National Laboratory. It provides
interactive GUI tools and a scripting API covering a large fraction of
Irena's analysis capabilities, using the same underlying algorithms and
scientific conventions. All data and results are stored as
`NXcanSAS <https://www.nexusformat.org/>`_-conformant HDF5 files, making
them self-contained and shareable across tools and collaborators.

The package is MIT-licensed, currently at **v0.7.0 (public beta)**, and
available on PyPI.

.. note::

   This page provides an overview. For detailed installation instructions,
   GUI guides, and scripting examples, see the
   `docs folder on GitHub <https://github.com/jilavsky/pyirena/tree/main/docs>`_.
   Full documentation including screenshots and worked examples is there.

When to use pyIrena vs. Irena
------------------------------

Use **Irena (Igor Pro)** when you need:

* The full set of Irena tools, including those not yet ported to Python
* Slit-smearing support throughout (USAXS data analysis)
* Tight integration with Indra and Nika data reduction
* An established, fully validated workflow

Use **pyIrena** when you need:

* A Python-only environment (no Igor Pro license)
* Batch processing and scripting via Python or Jupyter
* Integration with other Python scientific packages (NumPy, SciPy, matplotlib)
* AI/agent-assisted analysis via the built-in MCP server

Installation
------------

.. code-block:: bash

   pip install pyirena[gui]

This installs the package and all GUI dependencies (PySide6, pyqtgraph,
etc.). Python 3.10–3.13 is supported.

A conda-compatible environment file is also provided for those who prefer
to manage the scientific stack (NumPy, SciPy, h5py) via conda and the Qt6
GUI stack via pip. See
`installation.md <https://github.com/jilavsky/pyirena/blob/main/docs/installation.md>`_
for details.

Launching the GUI
-----------------

The primary entry point opens the Data Selector panel:

.. code-block:: bash

   pyirena-gui

Individual tools can also be launched directly:

.. code-block:: bash

   pyirena-viewer      # Data Explorer / HDF5 browser
   pyirena-modeling    # Modeling
   pyirena-datamerge   # Data Merge
   pyirena-contrast    # Scattering Contrast Calculator

Available analysis tools
-------------------------

The following Irena tools have been ported to pyIrena. The science and
algorithms are the same as in the Igor Pro version; the interface is a
native Python GUI built on Qt6.

**Modeling**
   Parametric forward-modeling combining up to 10 populations, each
   representing a size distribution, Unified level, or diffraction peak.
   Supports five size distribution functions (Gaussian, LogNormal, LSW,
   Schulz-Zimm, Ardell), nine form factors (sphere, spheroid, cylinder,
   core-shell variants), and two structure factors (Born-Green, Hard Sphere
   via Percus-Yevick). Monte Carlo uncertainty estimation is included.

**Unified Fit**
   Implements the Beaucage hierarchical scattering function with 1–5
   structural levels, each combining a Guinier region and a power-law tail,
   with optional Born-Green inter-particle correlation.

**Size Distribution**
   Indirect Fourier transform inversion of SAS data to recover particle
   size distributions. Four inversion methods: Maximum Entropy,
   Regularization, TNNLS, and Monte Carlo.

**Simple Fits**
   Thirteen direct analytical models: Guinier, Guinier-Porod, Porod,
   Sphere, Spheroid, Guinier Rod, Guinier Sheet, Debye-Bueche,
   Treubner-Strey, Power Law, and others. Each includes linearization
   plots and Monte Carlo uncertainty estimation.

**WAXS Peak Fit**
   Fits diffraction peaks using Gaussian, Lorentzian, Pseudo-Voigt, or
   Log-Normal profiles. Features automatic peak detection via Savitzky-Golay
   filtering and simultaneous polynomial background fitting.

**Data Merge**
   Merges two SAS datasets (e.g., SAXS + WAXS) onto a common Q scale
   using Nelder-Mead optimization for scale factor, flat background, and
   optional Q-shift — equivalent to Irena's "Merge two data sets" tool.

**Scattering Contrast Calculator**
   Computes X-ray and neutron scattering length densities from chemical
   formula and density, and calculates contrast (Δρ²) between two
   materials.

**Data Explorer (HDF5 browser)**
   Browses NXcanSAS HDF5 files, inspects raw data and analysis results,
   and supports export to Igor Pro h5xp format for round-tripping between
   Python and Igor Pro.

**Import Igor Pro experiment**
   Converts Igor Pro ``.pxp`` or ``.h5xp`` files to stand-alone NXcanSAS
   HDF5 files, enabling migration of legacy datasets without re-reducing
   from raw detector files.

Batch scripting API
--------------------

All tools support headless (no-GUI) execution via Python or JSON
configuration files. The core entry point is ``fit_pyirena()``, with
individual functions for each analysis type:

.. code-block:: python

   from pyirena.api import fit_unified, fit_sizes, fit_simple, merge_data

   result = fit_unified("mydata.h5", levels=2)

See
`batch_api.md <https://github.com/jilavsky/pyirena/blob/main/docs/batch_api.md>`_
for the full API reference and examples.

AI / MCP integration
---------------------

pyIrena ships a Model Context Protocol (MCP) server, installable with:

.. code-block:: bash

   pip install pyirena[mcp]

This exposes HDF5 readers, parameter aggregation, and headless plotting
to MCP-compatible clients (Claude Desktop, Claude Code, and custom
agents). Included tools include ``pyirena_summarize_folder``,
``pyirena_tabulate_parameter``, and ``pyirena_plot_iq``. See
`ai_integration.md <https://github.com/jilavsky/pyirena/blob/main/docs/ai_integration.md>`_
for setup and usage.

Data format
-----------

All pyIrena inputs and outputs use HDF5 files conforming to the NXcanSAS
standard. The same files can be read by SasView, Irena (via HDF5 Browser
or Import Igor Experiment), and any other NXcanSAS-compatible software.

Further documentation
----------------------

The `docs folder on GitHub <https://github.com/jilavsky/pyirena/tree/main/docs>`_
contains 25 markdown files covering:

* Installation and quick-start guides
* Per-tool GUI walkthroughs with screenshots
* Batch scripting and API reference
* HDF5 file structure reference
* AI/MCP integration guide
* Developer notes on adding form factors and structure factors

**Issues and feature requests:** https://github.com/jilavsky/pyirena/issues
