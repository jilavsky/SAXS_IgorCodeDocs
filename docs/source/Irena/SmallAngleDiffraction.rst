.. _model.small_angle_diffraction:

.. index::
     model; small-angle diffraction

Small-angle diffraction tool
============================

**Small-angle diffraction tool models data using :**

**Flat background**

**One Unified level (Guinier + Power law)**

**Up to 6 peaks**

Each peak can have one of many different shapes – Gauss, Lorenz, Pseudo-Voigt, Gumbel, Pearson-VII, modified Gauss, Lorenz-Squared, or Skewed Normal. Peaks can also represent Percus-Yevick S(q) structure factor and Percus-Yevic S(\ *q*) multiplied by Sphere F(\ *q*). Please note, that you should use **only** the shapes which are meaningful for your problem and you can justify. For example the S(q) and S(q)F(q) may be real challenge to justify inmost cases. I needed them for *very* specific case.

Some first have 3 parameters – prefactor (~intensity), position (NOTE: using Q units) and width (in Q units). Some have one more parameter which controls the tail height or some other shape features. Note, that for Pseudo-Voigt when eta = 0 the shape is Gauss and eta=1 the shape is Lorenzian.

The tool will manage slit-smeared data (USAXS data). There are few more details *very* important for slit-smeared data:

It is very useful to use experimental data which extend significantly beyond to slit length. If the data to less than slit length are used, it is important to model peaks which extend to Q positions smaller than slit length. If you see ripples (caused by slit smearing very narrow peaks), you can use “Oversample” checkbox – but that will increase the calculation time by about 5x.

The structure peak position ratios are collected from:

*Block copolymers: synthetic strategies, Physical properties and applications*, Hadjichrististidis, Pispas, Floudas, Willey & sons, 2003, chapter 19, pg 347.

Use of the tool:

Select “Small-angle diffraction” from the menu

.. figure:: media/SmallAngleDiffraction1.png
   :align: center
   :width: 100%


Select Data in the data selection controls and click graph button… Data
are graphed.

**Function of controls**

“auto recalculate” will cause data to be recalculated after most
parameter changes. If calculations take long time, you may want to
uncheck this and recalculate data using button “Recalculate”.

**VERY IMPORTANT**

*“Peak SAS rel.” – this is very important checkbox*. In case this
checkbox is NOT selected, the following is the formula to calculate
intensity:

.. math::

    I(Q)=I_{UnifiedFit}(Q)+ \sum_{i}I_{UnifiedFit}(Q)K_iF_i(Q)

While when it is checked, then the formula is:

.. math::

    I(Q)=I_{UnifiedFit}(Q)+ \sum_{i}K_iF_i(Q)


Where K\ :sub:`i` is scaling factor for each diffraction peak.

Where :math:`\Psi (Q)` is function of the three or four peak parameters – scaling factor, peak position, width, and for some also “tail” parameter. The exact formulas vary depending on peak profile selected.

**What does this mean? If the checkbox is NOT selected, the calculation is based on assumption, that the SAS scattering and diffraction peaks are from one population and loosely one can see it as F(Q)\*S(Q) assumption in small-angle scattering.**

**If the checkbox IS selected, the assumption is loosely that the peaks are independent of small-angle scattering and are produced by some other features than what produces the SAS itself.**

I suspect, that right selection is based on experience and what really fits right. Note, that the parameters are always evaluated for Ψ(Q) only… This is *VERY* important to understand and if you see cases, when these assumptions are wrong, please, let me know…

**Diffraction peaks profiles** are described in :ref:`Peak Profiles <DiffractionPeaksProfiles>`.

“Display peaks” will display individual peaks. Note, data for individual peaks are never smeared.

“Oversample” – for sit smeared data only. Will oversample Q range with 5x as many point to reduce artifacts caused by slit smearing very narrow
peaks.

Tab SAS:

G – prefactor for power law slope

P – power law slope

Bckg – flat backgroud

Tabs for Peaks:

.. figure:: media/SmallAngleDiffraction16.png
   :align: left
   :width: 300


“Use” – use the peak. No need to use peaks in order, can be mixed-and-matched

“Distribution type” – peak shape

“Prefactor” – scaling factor for the peaks (~hight)

“Position” – peak position in Q units

“width” – peak width in Q units

“Link Position to other peak?” – you can link peak position to position of another peak with scaling constant.

Lower set of parameters are peak parameters calculated numerically, so they may be slightly different than the numbers above.

Final controls:

.. figure:: media/SmallAngleDiffraction17.png
   :align: center
   :width: 380px


“Use genetic optimization?” – uses :ref:`genetic optimization <important.GeneticOptimization>`… Very slow fitting routine unlikely needed for this application. If needed, read explanation of the method in previous chapters.

“Fit” – fits

“Revert back” – reloads stored parameters from before fitting.

“Add tags to graph” – adds tags with parameters into the graph…

“Remove tags” – removes tags from the graph.

“Structure?” – sets ratios of positions for some known structures. Peak positions will be fixed with respect to Peak1. Note, user must set correct widths and prefactors for each peak manually…

.. figure:: media/SmallAngleDiffraction18.png
   :align: center
   :width: 75%


“Save in Fldr.” Saves results (including peak profiles if selected) back into data folder.

“Paste to Notebook” – opens notebook for results and pastes in there graph and summary of results.

.. figure:: media/SmallAngleDiffraction19.png
   :align: center
   :width: 90%


“Recalculate” – forces model recalculation if user needs to do it.

You can attach also residuals or normalized residuals into the graph, see example below.

.. figure:: media/SmallAngleDiffraction20.png
   :align: center
   :width: 90%


Useful comments:

Make sure the fitting parameters ranges are set appropriately. This is IMPORTANT and not obvious problem in fitting (experience speaks)… Results of fitting are also automatically recorded to into usual “SAS logbook” these tools keep… All is recorded there in more or less useful form. Your notes I keep for you....
