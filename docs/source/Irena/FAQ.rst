.. _faq:

.. index::
    FAQ Irena

Frequently asked questions Irena
================================

**Purpose**

These are questions which were asked by users over time, typically via e-mail. As they seem to be of general interest, they arelsited here. There is no specific order or system to these FAQs, so searchin a page for term you are aksing about is probably the best way of finding if there is an answer you are looking for. I will be updating these FAQs as they come.

List of Subjects
----------------

1.  :ref:`What to cite <citation>`
2.  :ref:`Do Modeling populations have requried order? <ModelingPopsOrder> `
3.  :ref:`What is RgCo in Unified fit and why do I need to understand it? <RgCO>`



FAQs:

.. _Citation:

*Citation to use in publications*

Jan Ilavsky and Peter R. Jemian, *“Irena: tool suite for modeling and analysis of small-angle scattering”*, Journal of Applied Crystallography, vol. 42 (2009).


.. _ModelingPopsOrder:

*Do modeling populations have required order?*

Unified fit tool requires specific order to "levels" used - level 1 must be smallest features in the syste, level 2 larger, and so on. This is bit challenging when a sequence of samples needs to be analyzed and new structure appears in teh middle of processing this sequence. Does the Modeling package have any requirements of the order of populations?

The simple answer is NO, it does not. You can have any order for Modeling populations as you wish. I personally start with setting pop1 to most dominating system and then keep adding the populations of scatterers as needed. Keep in mind, that this means MOdeling cannot have some nice features of Unified fit (linking of RgCO parameter) and that you need to keep much better track on what teh sizes do as you do fitting. *I strongly suggest to use field "What is this" for each population to keep track of them*.


.. _RgCO:

*What is RgCo in Unified fit and why do I need to understand it?*

RgCO is suprisingly important parameter for Unified fit which is poorely understood by many or even most users. It causes a lot of problems if it is applied incorrectly. 
