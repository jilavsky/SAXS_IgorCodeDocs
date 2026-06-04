.. _irena-modeling-models:
.. _model.models:

.. index::
    model; Modeling Populations models

Model description
==================

This chapter describes the mathematical models available for scattering
populations in the Modeling package. Any population can use one of the
following types:

 | **"Size distribution"**
 | **"Unified level"**
 | **"Surface fractal"**
 | **"Mass Fractal"**
 | **"Diffraction peak"**

This flexibility allows modeling of complex small-angle scattering data. The
Small-angle diffraction tool and the Fractals tool also use some of these models.

Size distribution
-----------------

Size distribution parameters are described on the main Modeling package page.
The size distribution can use any of the form factors and structure factors
available in Irena. See: :ref:`Form and Structure factors <FormStructureFactors>`.
For GUI details, assumptions, and distribution shapes, see:
:ref:`Size distribution <SizeDistributionDescription>`.

Unified Fit
-----------

The Unified Fit uses the formula explained in the Unified Fit model page:
:ref:`Unified Fit <unified-fit>`.

.. _DiffractionPeaksProfiles:

Diffraction Peaks
-----------------

Diffraction peak shapes are used in the Small-Angle Diffraction tool and in
the Modeling package. The following formulas define the peak profile Ψ(x)
for each available shape:

1. **Gaussian Function**

.. math::

    \Psi(x)=M \cdot \exp\!\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)

where σ is the Gaussian width, μ is the peak center, and M is the scaling factor.

2. **Modified Gaussian Function**

.. math::

    \Psi(x)=M \cdot \exp\!\left(-\frac{(x-\mu)^d}{2\sigma^d}\right)

where d ≥ 1 is the exponent controlling the falloff rate.

3. **Lorentz Function** (Lorentz-squared is this function squared)

.. math::

    \Psi(x)=M \cdot \frac{a}{\pi(a^2+(x-\mu)^2)}

where *a* is the Lorentzian width.

4. **Pseudo-Voigt Function**

.. math::

      \Psi(x)=M \cdot \left(\eta\frac{1}{1+x^2}+(1-\eta)\exp(-(ln2)x^2)\right)

      x= \frac{2(x-x_0)}{w}

where x\ :sub:`0` is the peak center, w is the FWHM, and 0 ≤ η ≤ 1 is the
weight parameter. η = 0 gives a pure Gaussian; η = 1 gives a pure Lorentzian.

5. **Pearson type VII Function**

.. math::

    \Psi(x)=M \cdot \left[1+\frac{(x-\mu)^2}{ma^2}\right]^{-m}

where *a* is proportional to the FWHM and *m* controls the tail falloff rate.

6. **Gumbel Function**

.. math::

    \Psi(x)=\frac{1}{\beta}\exp\!\left(\frac{x-\mu}{\beta}\right)\exp\!\left(-\exp\!\left(\frac{x-\mu}{\beta}\right)\right)

where β is the width and μ is the peak center.

7. **Skew normal function**

.. Figure:: media/SmallAngleDiffraction15.png
   :align: center
   :width: 780px

8. **Percus-Yevick S(q)** and **Percus-Yevick S(q) × Sphere F(q)** — described
   in the Form factor and Structure factor PDF (accessible from the SAS menu in
   Igor Pro). The P-Y S(q) code is derived from the NIST SANS data analysis
   macros.

.. _MassAndSurfaceFractals:

Surface and Mass Fractal
------------------------

This model was developed for analysis of fractal systems in cement, see:
https://www.nature.com/articles/nmat1871. For more details, see also:
:ref:`Fractal model <model.Fractal>`. When possible, use the dedicated Fractals
tool — it is simpler to use than configuring this population type in Modeling.

The model predicts Q\ :sup:`Dv` scattering (between Q\ :sup:`-1` and Q\ :sup:`-3`)
for mass or volume fractals, and Q\ :sup:`6-Ds` scattering (between Q\ :sup:`-3`
and Q\ :sup:`-4`) for surface fractals.

The full model for dΣ/dΩ as a function of Q contains four components:

    dΣ/dΩ = {VOLUME FRACTAL + SINGLE GLOBULE} TERM + SURFACE FRACTAL + FLAT BACKGROUND

These components are incorporated into the full theoretical expression as follows:

.. Figure:: media/FractalsModels1.jpg
        :width: 100%

The first volume-fractal term contains Φ\ :sub:`CSH`, ξ\ :sub:`v`, and the
mean radius R\ :sub:`o` and shape aspect ratio β of the building-block C-S-H
gel globules in the volume-fractal phase (assumed to be spheroids). It also
contains a local volume fraction η and the mean correlation-hole radius
R\ :sub:`c` — the mean nearest-neighbor separation of gel-globule centers.
R\ :sub:`c`, weighted over spheroid surface contacts, is given by:

.. Figure:: media/FractalsModels2a.jpg
        :width: 70%

.. Figure:: media/FractalsModels2b.jpg
        :width: 70%

The single-globule term arises because nearest-neighbor solid particles cannot
overlap (centers cannot approach within R\ :sub:`c`). This correlation-hole
effect makes individual particles distinguishable even within an aggregated
structure, at length scales of order R\ :sub:`o`. For a spheroid of aspect ratio
β, the single-globule form factor F\ :sup:`2`(Q) is given by:

.. Figure:: media/FractalsModels3.jpg
        :width: 80%

where V\ :sub:`p` = (4βπR\ :sub:`o`/3), J\ :sub:`3/2`(x) is a Bessel function
of order 3/2, and X is an orientational parameter integrated over all
orientations of the spheroid with respect to Q. A mildly spheroidal shape avoids
the pronounced Bessel function oscillations for perfect spheres (β = 1) that
can perturb fits at high Q. Both mildly oblate (β = 0.5) and mildly prolate
(β = 2) shapes give equivalent fits for cement systems.

The surface fractal term includes ξ\ :sub:`s`, the upper limit of surface-fractal
behavior, and S\ :sub:`o`, the measured smooth surface area per unit sample
volume. The term Γ(5 − D\ :sub:`s`) is the mathematical gamma function.

The BACKGROUND term represents incoherent flat background scattering, usually
subtracted from both data and fits for convenience.
