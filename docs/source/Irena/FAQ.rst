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
2.  :ref:`Do Modeling populations have required order? <FAQ.ModelingPopsOrder>`
3.  :ref:`What do I do if I need different Form Factor? <FAQ.ModelingUserFormFactor>`
4.  :ref:`Why does Modeling warn against use of multiple Unified levels? <FAQ.ModelingMultipleUFlevels>`
5.  :ref:`What is RgCo in Unified fit and why do I need to understand it? <FAQ.RgCO>`
6.  :ref:`How does Irena get volume fraction (and other absolute values)? <FAQ.Calibration>`




FAQs:
=====

.. _FAQ.Citation:

*Citation to use in publications*
---------------------------------
Jan Ilavsky and Peter R. Jemian, *“Irena: tool suite for modeling and analysis of small-angle scattering”*, Journal of Applied Crystallography, vol. 42 (2009).

.. _FAQ.ModelingPopsOrder:

*Do modeling populations have required order?*
----------------------------------------------
Unified fit tool requires specific order to "levels" used - level 1 must be smallest features in the system, level 2 larger, and so on. This is bit challenging when a sequence of samples needs to be analyzed and a new structural level appears in the middle of processing this sequence. Does the Modeling package have any requirements of the order of populations?

The simple answer is NO, it does not. You can have any order for Modeling populations as you wish. I personally start with setting pop1 to most dominating system and then keep adding the populations of scatterers as needed. Keep in mind, that this means Modeling cannot have some nice features of Unified fit (linking of RgCO parameter) and that you need to keep much better track on what the sizes do as you do fitting. *I strongly suggest to use field "What is this" for each population to keep track of them*.

.. _FAQ.ModelingMultipleUFlevels:

*What do I do if I need different Form Factor?*
-----------------------------------------------
If you need different Form Factor than is included in Irena, you can write your own functions. Alternatively, you can modify Form Factors (or Structure Factors) included in NIST Igor data analysis package. If you need help, you can contact me and I can help, especially if the Form Factor function is included in the NIST package. :ref:`More details are available here <FormFactors.User>`

*Why does Modeling warn against use of multiple Unified levels?*
----------------------------------------------------------------
Unified fit tool requires specific order to "levels" used but Modeling does not, see above. This specific order of levels enables the code to do some background checks for sensibility of a solution. It also enables users to conveniently link RgCO (see below) together as needed. None of this is possible in the Modeling tool. It is therefore strongly recommended to use Unified fit for fitting of data requiring multiple Unified levels - and not use Modeling tool for this purpose. For complex systems this may not be possible, though... There is nothing inherently wrong in using multiple Unified levels in Modeling tool, it is just more complicated and error prone.


.. _FAQ.RgCO:

*What is RgCo in Unified fit and why do I need to understand it?*
-----------------------------------------------------------------

RgCO is surprisingly **important** parameter for Unified fit which is poorly understood by many or even most users. It causes a lot of problems if it is applied incorrectly.

Many structures studied in small-angle scattering exhibit multiple length scales on which they scatter (not everything is a sphere) - e.g., just simple cylinder with aspect ration >1 has two major length scales - radius and length. Both of these generate in small angle scattering something, which resembles Guinier Knee, with its own Rg :ref:`Cylinder form factor <FormFactors.Cylinder>`. In order to model such system in Unified fit we need to use multiple levels for one system of particles. In fitting you start with smaller dimension (radius for the cylinder) and fit that with a level (let's say level 1). This fits the high-q area. Then you can fit the low-q area with Level 2; you fit the Guinier area and then power law slope between the two Rgs. But this power law slope is approximately -1 (see :ref:`Cylinder form factor <FormFactors.Cylinder>`). This means that Level 2 power law scattering will eventually be higher that SAXS data. To "merge" tow levels into one and allow these two levels to represent ONE population of scatterers - which scatter at multiple length scales - you need to employ the RgCO parameter. This parameter needs to be set for Level 2 and set to value of Rg for Level 1. In Unified fit there is helpful checkbox which links the RgCo of level n to Rg of level n-1. Note, that you can have multiple length scales for scattering system well beyond two - three dimensional object may have three, but hierarchical systems like fractals can have even more. It is, however, unlikely it would be possible to collect SAXS/SANS data which reflect this. Please also check :ref:`Do Modeling populations have required order? <FAQ.ModelingPopsOrder>`.

.. _FAQ.Calibration:

*How do Irena tools get absolute volume fractions, specific sufc areas etc?*
----------------------------------------------------------------------------

Irena expects that input data are properly calibrated. Therefore, the Q vector is expected to be calibrated and have units of [1/Angstrom]. This is common and expected by users...

However, in order to get absolute volume fractions or specific surface areas of scatterers, Irena needs input INTENSITY data to be on absolute intensity scale. Irena expects the Intensity to be in [cm2/(cm3\*steradian)], usually noted as [cm2/cm3] or even [1/cm]. If such data are provided and *if user inputs correct scattering contrast*, Irena output is correct volume fraction. For this, see paper on Glassy carbon: Allen, Andrew J., Fan Zhang, R. Joseph Kline, William F. Guthrie, and Jan Ilavsky. "NIST Standard Reference Material 3600: Absolute Intensity Calibration Standard for Small-Angle X-Ray Scattering." Journal Of Applied Crystallography 50, no. 2 (Apr 1 2017): 462-74. http://dx.doi.org/doi:10.1107/S1600576717001972. There are many papers on this subject, my movie is on Youtube (https://www.youtube.com/watch?v=FM5w2hwT7Ns&list=PL_su_4DtkZp_DCXqXX-jmo5upybkAKE3B) etc. Basically, Irena expects that your data are corrected for sample thickness and all instrument effects. If needed, check Nika part of this manual.
