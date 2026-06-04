.. _nika-introduction:
.. _Introduction_Nika:

.. index:: Nika; Introduction

Introduction
============

Manual |release| for Nika version 1.75 for Igor Pro 9.05 and higher.

|today|

**Jan Ilavsky**

If you use Nika in published work, please cite:

   Jan Ilavsky, "Nika — software for 2D data reduction", *J. Appl. Cryst.*
   (2012), vol. 45, pp. 324–328. DOI: 10.1107/S0021889812004037.

Description
-----------

This is the manual for the **Nika** set of macros developed for Igor Pro
(WaveMetrics, Inc., `www.wavemetrics.com <http://www.wavemetrics.com>`__),
Igor Pro 9.05 and higher. These macros process 2D data from CCD and other area
detectors used in small-angle and wide-angle scattering instruments. The goal
is to normalize, background-correct, and calibrate 2D data from an experiment
and convert them into 1D profiles (intensity, Q or 2θ or d, and errors).

Nika provides the following methods for extracting 1D data:

#. Sector and circular averages ("cake")
#. Intensity along linear and elliptical paths (vertical/horizontal lines,
   lines at arbitrary angle, and ellipses of arbitrary aspect ratio)
#. Intensity along a linear path in grazing-incidence geometry
#. Intensity versus azimuthal angle (for manual geometry inspection)

.. note::

   These macros represent a collaborative work in progress and not all features
   may be complete at any given time. While every effort is made to verify
   results, no guarantees can be made as to their reliability. Please verify
   results independently and report any bugs to ilavsky@aps.anl.gov. Support
   is provided on a best-effort basis.
