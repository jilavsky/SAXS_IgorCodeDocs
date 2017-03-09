Introduction
============


Jan Ilavsky, "Nika - software for 2D data reduction", J. Appl. Cryst. (2012), vol. 45, pp. 324-328. DOI:10.1107/S0021889812004037. Please e-mail me, if you need copy.

Manual |release| for Nika version 1.75 for Igor 7.x

|today|

**Jan Ilavsky**

Description
-----------

This is manual for set of macros developed for Igor Pro (Wavemetrics, Inc, `www.wavemetrics.com <http://www.wavemetrics.com>`__) Igor 7.0x. These macros are designed to process 2D (CCD and other area detectors) data from small-angle and wide-angle scattering instruments. The purpose is to process (normalize, background correct, calibrate,...) 2D data from experiment and convert these into 1D “line outs” data – providing correctly calibrated Intensity, q (two theta or d), and errors.

Nika was designed to provide number of methods to extract the data:

#. Method of Sector and circular averages

#. Intensity along linear and elliptical path (vertical/horizontal lines, line under and angle and ellipse of arbitrary aspect ratio)

#. Intensity along linear path but for Grazing incidence geometry

#. Intensity vs azimuthal angle image intended for manual inspection of geometry.

Disclaimer:

These macros represent a collaborative work in progress and it is very
likely that not all features are finished at any given time. Therefore,
some features may not work fully or at all. Please note, while I try my
best to verify the results, no guarantees can be made as to the
reliability of these results. Please, verify results in some other way.
Please report any bugs to me, I will do my best to fix them ASAP. I
provide limited support for users of these macros. Limited means that my
time available for this support is limited. If you need help, e-mail
Igor file to me with data so I can work on your data.

ilavsky@aps.anl.gov
