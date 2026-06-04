.. _irena-faq:
.. _faq:

.. index::
    Irena; FAQ

Frequently asked questions — Irena
====================================

This section collects questions that have been asked by users over time,
typically by email, where the answers are of general interest. There is no
specific ordering; searching this page for a keyword is the most efficient
way to find a relevant answer.

List of subjects
-----------------

1.  :ref:`What to cite <FAQ.Citation>`
2.  :ref:`Do Modeling populations have a required order? <FAQ.ModelingPopsOrder>`
3.  :ref:`Why does Modeling warn against using multiple Unified levels? <FAQ.ModelingMultipleUFlevels>`
4.  :ref:`What is RgCo in Unified fit and why does it matter? <FAQ.RgCO>`
5.  :ref:`How does Irena obtain volume fraction and other absolute values? <FAQ.Calibration>`

FAQs
-----

.. _FAQ.Citation:

Citation
~~~~~~~~

Jan Ilavsky and Peter R. Jemian, *"Irena: tool suite for modeling and analysis
of small-angle scattering"*, Journal of Applied Crystallography, vol. 42 (2009).

.. _FAQ.ModelingPopsOrder:

Do modeling populations have a required order?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Unified Fit tool requires a specific ordering of levels — level 1 must
correspond to the smallest features, level 2 to the next larger, and so on.
This can be challenging when a new structural level appears partway through
processing a series of samples. Does the Modeling package have a similar
ordering requirement?

The answer is **no**. Modeling populations can be arranged in any order.
A recommended approach is to assign population 1 to the dominant scattering
component and add further populations as needed. Because there is no required
ordering, Modeling cannot offer some of the conveniences of Unified Fit (such
as automatic RgCO linking), and careful tracking of what each population
represents is essential. *The "What is this" field for each population is
strongly recommended for this purpose.*

.. _FAQ.ModelingMultipleUFlevels:

What do I do if I need a different Form Factor?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If the form factor you need is not included in Irena, you can write your own
function. Alternatively, you can adapt form factors or structure factors from
the NIST Igor data analysis package. Contact the developer for assistance —
particularly if the form factor is already implemented in the NIST package.
:ref:`More details are available here. <FormFactors.User>`

Why does Modeling warn against using multiple Unified levels?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Unified Fit requires a specific level ordering (see above) which enables the
code to perform internal consistency checks and to automatically link the RgCO
parameter between adjacent levels (see below). Neither of these capabilities
is available in the Modeling tool. It is therefore strongly recommended to use
the Unified Fit tool when multiple Unified levels are needed — not the Modeling
tool, as this is more complex and more prone to error.

For systems where Unified Fit is not sufficient (e.g., a combination of Unified
levels with size distributions or diffraction peaks), multiple Unified levels
in the Modeling tool can be used, but extra care is required.

.. _FAQ.RgCO:

What is RgCo in Unified Fit and why does it matter?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

RgCO is a **critical** parameter in Unified Fit that is frequently misunderstood.
Incorrect use leads to physically meaningless fits.

Many structures studied by small-angle scattering scatter at multiple length
scales simultaneously — even a simple cylinder with aspect ratio > 1 has two
distinct length scales (radius and length), each producing a Guinier-like
feature with its own R\ :sub:`g`. See :ref:`Cylinder form factor <FormFactors.Cylinder>`.

To model such a system in Unified Fit, two levels are used. Level 1 fits the
smaller dimension (e.g., radius at high Q) and Level 2 fits the larger
dimension (e.g., length at low Q), including the power-law slope between the
two Guinier regions (approximately −1 for a cylinder). However, the power law
of Level 2 will eventually exceed the measured scattering data at high Q.

To merge the two levels into a single physically meaningful representation of
one population of scatterers, the RgCO parameter of Level 2 must be set equal
to the R\ :sub:`g` value of Level 1. Unified Fit provides a checkbox to link
RgCO of level n to the R\ :sub:`g` of level n−1 automatically.

The same principle applies to systems with more than two length scales.
See also: :ref:`Do Modeling populations have a required order? <FAQ.ModelingPopsOrder>`

.. _FAQ.Calibration:

How does Irena obtain absolute volume fractions and specific surface areas?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Irena expects properly calibrated input data:

* The Q vector must be calibrated in units of Å\ :sup:`-1`.
* For absolute volume fractions or specific surface areas, the intensity must
  be on absolute scale in units of cm\ :sup:`2`/(cm\ :sup:`3`·sr), commonly
  written as cm\ :sup:`2`/cm\ :sup:`3` or cm\ :sup:`-1`.

When absolute intensity data are provided and the correct scattering contrast
is entered, Irena outputs correct volume fractions.

For absolute calibration using Glassy Carbon SRM 3600, see:

   Allen, A. J., Zhang, F., Kline, R. J., Guthrie, W. F., and Ilavsky, J.
   "NIST Standard Reference Material 3600: Absolute Intensity Calibration
   Standard for Small-Angle X-Ray Scattering." *Journal of Applied
   Crystallography* 50(2) (2017): 462–474.
   http://dx.doi.org/doi:10.1107/S1600576717001972

A video tutorial on absolute calibration is available on the Irena YouTube
channel: https://www.youtube.com/watch?v=FM5w2hwT7Ns

Irena expects input data that are fully corrected for sample thickness and all
instrument effects. If needed, refer to the Nika section of this manual.
