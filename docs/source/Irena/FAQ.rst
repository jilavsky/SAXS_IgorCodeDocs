.. _faq:

.. index::
    FAQ Irena

Frequently asked questions Irena
================================

**Purpose**

These are questions which were asked by users over time, typically via e-mail. As they seem to be of general interest, they are listed here. There is no specific order or system to these FAQs, so search in a page for term you are asking about is probably the best way of finding if there is an answer you are looking for. I will be updating these FAQs as they come.

List of Subjects
----------------

1.  :ref:`What to cite <FAQ.citation>`
2.  :ref:`Do Modeling populations have requried order? <FAQ.ModelingPopsOrder>`
3.  :ref:`What is RgCo in Unified fit and why do I need to understand it? <FAQ.RgCO>`



FAQs:
=====

.. _FAQ.Citation:

*Citation to use in publications*
---------------------------------
Jan Ilavsky and Peter R. Jemian, *“Irena: tool suite for modeling and analysis of small-angle scattering”*, Journal of Applied Crystallography, vol. 42 (2009).



.. _FAQ.ModelingPopsOrder:

*Do modeling populations have required order?*
----------------------------------------------
Unified fit tool requires specific order to "levels" used - level 1 must be smallest features in the syste, level 2 larger, and so on. This is bit challenging when a sequence of samples needs to be analyzed and new structure appears in teh middle of processing this sequence. Does the Modeling package have any requirements of the order of populations?

The simple answer is NO, it does not. You can have any order for Modeling populations as you wish. I personally start with setting pop1 to most dominating system and then keep adding the populations of scatterers as needed. Keep in mind, that this means MOdeling cannot have some nice features of Unified fit (linking of RgCO parameter) and that you need to keep much better track on what teh sizes do as you do fitting. *I strongly suggest to use field "What is this" for each population to keep track of them*.



.. _FAQ.RgCO:

*What is RgCo in Unified fit and why do I need to understand it?*
-----------------------------------------------------------------

RgCO is suprisingly **important** parameter for Unified fit which is poorely understood by many or even most users. It causes a lot of problems if it is applied incorrectly.

Many structures studied in small-angle scattering exhibit multiple lengthscales on which they scatter (not everything is a sphere) - e.g., just simple cylinder with aspect ration >1 has two major lengthscales - radius and length. Both of these generate in small angle scattering something, which resembles Guinier Knee, with its own Rg :ref:`Cylinder form factor <FormFactors.Cylinder>`. In order to model such system in Unified fit we need to use multiple lelves for one system of particles. In fitting you start with smaller dimension (radius for the cylinder) and fit that with a level (let's say level 1). This fits the high-q area. Then you can fit the low-q area with Level 2; you fit the Guinier area and then power law slope between the two Rgs. But this power law slope is approximately -1 (see :ref:`Cylinder form factor <FormFactors.Cylinder>`). This means that Levle 2 power law scattering will eventually be higher that SAXS data. To "merge" tow levels into one and allow these two levels to represent ONE population of scatterers - which scatter at multiple length scales - you need to employ the RgCO aprameter. This parameter needs to be set for Level 2 and set to value of Rg for Level 1. In Unified fit there is helpful chebox which links the RgCo of level n to Rg of level n-1. Note, that you can have multiple lengthscales for scattering system well beyond two - three dimensional object may have three, but hierarchical systems like fractals can have even more. It is, however, unlikely it would be possible to collect SAXS/SANS data which reflect this. Please also check :ref:`Do Modeling populations have requried order? <FAQ.ModelingPopsOrder>`.
