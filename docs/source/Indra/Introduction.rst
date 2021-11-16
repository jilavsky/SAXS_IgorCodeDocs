.. _introduction:

.. index::
    Introduction Indra (USAXS data reduction)

Introduction
============

This is manual for Indra package for data reduction of USAXS data collected on 9ID-C USAXS/SAXS/WAXS instrument at the Advanced Photon Source (APS) of Argonne National Laboratory. For more details see https://usaxs.aps.anl.gov

Manual |release| for Indra version 1.93 for Igor 8.04 and above

|today|

**Jan Ilavsky**

Disclaimer:

These macros represent a collaborative work in progress and it is very likely that not all features are finished at any given time. Therefore, some features may not work fully or at all. Please note, while I try my best to verify the results, no guarantees can be made as to the reliability of these results. Please, verify results in some other way. Please report any bugs to me, I will do my best to fix them ASAP. I provide limited support for users of these macros. Limited means that my time available for this support is limited. If you need help, e-mail Igor file to me with data so I can work on your data.

ilavsky@aps.anl.gov

Description
-----------

The “\ *Indra*\ ” package is a suite of Igor Pro (Wavemetrics, version Igor 7.05 and higher) macros for data reduction of small-angle scattering data collected on APS USAXS instrument (currently beamline 9ID, Advanced Photon Source, Argonne, IL).

.. index::
    USAXS instrument description

What is USAXS/SAXS/WAXS instrument?
-----------------------------------

Details on USAXS instrument are in following publications:
     1. Ilavsky, I., P. Jemian, A. J. Allen and G. G. Long (2004). Versatile USAXS (Bonse-Hart) facility for advanced materials research. Synchrotron Radiation Instrumentation. T. Warwick, J. Arthur, H. A. Padmore and J. Stohr. Melville, Amer. Inst. Physics. 705: 510-513.
     2. Ilavsky, J., P. R. Jemian, A. J. Allen, F. Zhang, L. E. Levine and G. G. Long (2009). "Ultra-small-angle X-ray scattering at the Advanced Photon Source." Journal of Applied Crystallography 42(3): 469-479.
     3. Zhang, F., A. J. Allen, L. E. Levine, J. Ilavsky and G. G. Long (2011). "Ultra-Small-Angle X-ray Scattering—X-ray Photon Correlation Spectroscopy: A New Measurement Technique for In-Situ Studies of Equilibrium and Nonequilibrium Dynamics." Metallurgical and Materials Transactions A 43(5): 1445-1453.
     4. Zhang, F., A. J. Allen, L. E. Levine, J. Ilavsky, G. G. Long and A. R. Sandy (2011). "Development of ultra-small-angle X-ray scattering–X-ray photon correlation spectroscopy." Journal of Applied Crystallography 44(1): 200-212.
     5. Ilavsky, J., A. J. Allen, L. E. Levine, F. Zhang, P. R. Jemian and G. G. Long (2012). "High-energy ultra-small-angle X-ray scattering instrument at the Advanced Photon Source." Journal of Applied Crystallography 45(6): 1318-1320.
     6. Ilavsky, J., F. Zhang, A. J. Allen, L. E. Levine, P. R. Jemian and G. G. Long (2013). "Ultra-Small-Angle X-ray Scattering Instrument at the Advanced Photon Source: History, Recent Development, and Current Status." Metallurgical and Materials Transactions a-Physical Metallurgy and Materials Science 44A(1): 68-76.

The USAXS/SAXS/WAXS instrument provides world-unique capabilities - or more details see https://usaxs.aps.anl.gov :

Standard configuration (Si 220 crystals)

  * Energy range ..........   10 - 24 keV
  * Q range ...................    0.0001 to 6 [1/A] (max depends on energy used)
  * Collection time ........  2 - 5 minutes
  * Intensity range ........  up to 12 decades (depends on sample), desmeared
  * Q resolution ............  ~ 0.00008 [1/A] in the USAXS range (up to ~0.1 [1/A])
  * SAXS ....................... Q range ~ 0.03 to 1.3 [1/A]
  * WAXS ...................... d spacing range approximately 6A to 0.8 A, depending on energy


High resolution configuration (Si 440 crystals) - requires significant setup and alignment time, special request only

  * Energy ....................  10 - 18 keV
  * Q range ...................   0.00003 to 6 [1/A] (max depends on energy used)
  * Collection time ........ 4 - 6 minutes
  * Intensity range ........  up to 12 decades (depends on sample), desmeared
  * Q resolution ............  ~ 0.00003 [1/A] in the USAXS range (up to ~0.1 [1/A])
  * SAXS ....................... Q range ~0.03 - 1.3 [1/A]
  * WAXS ...................... d spacing range approximately 6A to 0.8 A, depending on energy

.. index::
    USAXS naming system

When using Indra package to reduced data, it uses "USAXS" naming system for use with Irena macros. When you use Irena to analyze, plot, or even only export data, you need to select "USAXS" choice in the top of the panels.

**Make sure you save Igor experiment**
Igor is quite reliable and crashes rarely. But no software is crash proof and you really do not want to loose lot of your work and time. Therefore, do yourself a favor and save your Igor experiment, routinely.
**YOU WERE WARNED!!!!**
