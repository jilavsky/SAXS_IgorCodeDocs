.. _indra-introduction:
.. _introduction:

.. index::
    Indra; Introduction

Indra — Introduction
====================

.. sidebar:: Manual version

   Manual |release| for Indra version 2.05, Igor Pro 9.05 and above.

   |today|

   **Jan Ilavsky** — ilavsky@aps.anl.gov

This is the manual for USAXS instrument data collection and reduction
(*Indra* package) for the USAXS/SAXS/WAXS instrument, currently located at
beamline 12ID-E (C) at the Advanced Photon Source (APS), Argonne National
Laboratory. For more details see https://usaxs.xray.aps.anl.gov

.. note::

   These macros represent a collaborative work in progress and not all
   features may be complete at any given time. While every effort is made to
   verify results, no guarantees can be made as to their reliability. Please
   verify results independently, and report any bugs to ilavsky@aps.anl.gov.
   Support for users is provided on a best-effort basis.

----

What is the USAXS/SAXS/WAXS instrument?
----------------------------------------

.. index::
    USAXS instrument description

The *Indra* package is a suite of Igor Pro (WaveMetrics, version 9.05 and
higher) macros for data reduction of small-angle scattering data collected on
the APS USAXS instrument (currently beamline 12ID, Advanced Photon Source,
Argonne, IL).

Details on the USAXS instrument are available in the following publications:

1. Ilavsky, I., P. Jemian, A. J. Allen and G. G. Long (2004). Versatile USAXS (Bonse-Hart) facility for advanced materials research. *Synchrotron Radiation Instrumentation*. 705: 510–513.
2. Ilavsky, J., P. R. Jemian, A. J. Allen, F. Zhang, L. E. Levine and G. G. Long (2009). Ultra-small-angle X-ray scattering at the Advanced Photon Source. *Journal of Applied Crystallography* 42(3): 469–479.
3. Zhang, F., A. J. Allen, L. E. Levine, J. Ilavsky and G. G. Long (2011). Ultra-Small-Angle X-ray Scattering—X-ray Photon Correlation Spectroscopy: A New Measurement Technique for In-Situ Studies of Equilibrium and Nonequilibrium Dynamics. *Metallurgical and Materials Transactions A* 43(5): 1445–1453.
4. Zhang, F., A. J. Allen, L. E. Levine, J. Ilavsky, G. G. Long and A. R. Sandy (2011). Development of ultra-small-angle X-ray scattering–X-ray photon correlation spectroscopy. *Journal of Applied Crystallography* 44(1): 200–212.
5. Ilavsky, J., A. J. Allen, L. E. Levine, F. Zhang, P. R. Jemian and G. G. Long (2012). High-energy ultra-small-angle X-ray scattering instrument at the Advanced Photon Source. *Journal of Applied Crystallography* 45(6): 1318–1320.
6. Ilavsky, J., F. Zhang, A. J. Allen, L. E. Levine, P. R. Jemian and G. G. Long (2013). Ultra-Small-Angle X-ray Scattering Instrument at the Advanced Photon Source: History, Recent Development, and Current Status. *Metallurgical and Materials Transactions A* 44A(1): 68–76.
7. Ilavsky, J., Zhang, F., Andrews, R. N., Kuzmenko, I., Jemian, P. R., Levine, L. E., Allen, A. J. (2018). Development of combined microstructure and structure characterization facility for in situ and operando studies at the Advanced Photon Source. *Journal of Applied Crystallography* 51, 867–882. https://doi.org/10.1107/S160057671800643X
8. Zhang, F. and Ilavsky, J. (2024). Bridging length scales in hard materials with ultra-small angle X-ray scattering — a critical review. *IUCrJ* 11. https://doi.org/10.1107/S2052252524006298

The USAXS/SAXS/WAXS instrument provides world-unique measurement capabilities.
For full details see https://usaxs.xray.aps.anl.gov

**Standard configuration (Si 220 crystals) — flyscanning or step scanning**

* Energy range: 10–28 keV
* Q range: 0.0001 to 6 Å\ :sup:`−1` (maximum depends on energy)
* Collection time: 1–3 minutes
* Intensity range: up to 12 decades (desmeared)
* Q resolution: ~0.00008 Å\ :sup:`−1` in the USAXS range (up to ~0.1 Å\ :sup:`−1`)
* SAXS Q range: ~0.03 to 1.3 Å\ :sup:`−1`
* WAXS d-spacing range: ~6 Å to 0.8 Å (energy dependent)

**High resolution configuration (Si 440 crystals) — step scanning only**

* Energy: 20 or 24 keV
* Q range: 0.00003 to 6 Å\ :sup:`−1` (maximum depends on energy)
* Collection time: 4–6 minutes
* Intensity range: up to 12 decades (desmeared)
* Q resolution: ~0.00003 Å\ :sup:`−1` in the USAXS range (up to ~0.1 Å\ :sup:`−1`)
* SAXS Q range: ~0.03 to 1.3 Å\ :sup:`−1`
* WAXS d-spacing range: ~6 Å to 0.8 Å (energy dependent)

Data naming conventions
~~~~~~~~~~~~~~~~~~~~~~~

.. index::
    USAXS naming system

When using the USAXS instrument data reduction packages *Matilda* (automatic),
*Indra*, and *Nika* (manual), reduced data are available in Igor Pro using the
following naming conventions:

* ``USAXS`` — for USAXS data. Both slit-smeared and desmeared versions are
  typically available; the desmeared version is used for most analyses.
* ``QRS`` — for SAXS and WAXS data.

When using *Irena* to analyze, plot, or export data, select the appropriate
``USAXS`` or ``QRS`` choice at the top of each panel.

Instrument history
~~~~~~~~~~~~~~~~~~

This instrument was built at the Advanced Photon Source in 1998 at Beamline
33ID (UNICAT). It was subsequently relocated to beamline 32ID (~2005),
15ID — ChemMatCARS (~2010), 9ID (~2015), 20ID (2021, temporarily), and its
current location at 12ID since the APS-U upgrade in 2024. Additional details
are available at DOI: 10.1007/s11661-012-1431-y.
