Pair distance distribution function
===================================

**Model description**

This tool calculates Pair distance distribution function as generally
defined in the small-angle scattering theory (see any basic SAS book,
like Glatter/Kratky 1982, page 27, formula 29):

.. math::

      I(Q)=(\Delta\rho)^2V\int_{0}^{D}4\pi r^2dr \gamma_0(r)\frac{sin(Qr)}{Qr}=(\Delta\rho)^2V\sum_{0}^{D}4\pi r^2 \Delta r \gamma_0(r)\frac{sin(Qr)}{Qr}

where the :math:`(\Delta \rho)^2V`is contrast/volume of the scatterers (simply scaling) factor. This one is neglected in this tool and set to 1. This is how GNOM does it also.

The PDDF is :math:`\gamma_0(r)` and r is the distance (in A).

This tool uses two different methods to achieve its goal – Regularization (by Pete Jemian) and Moore’s method (Moore, P. B. (1980). J. Appl. Cryst. 13, 168–175.). Both seem to have some advantages and disadvantages. The tool was tested against generally accepted GNOM by D. I. Svergun (EMBL). Test cases available to me yield same results and GNOM.

From version 2.41 the tool also generate Gamma function: :math:`\gamma = \frac{PDDF}{4\pi r^2}`. Per user request. I am not sure what else to do with this, so if you want to get more functionality, let me know.

NOTE: I have observed significant changes on the calculated PDDF with changes with maximum dimension assumed and with errors scaling. My observations are noted in text further below.

Comment on graph formatting… This tool uses same default font and font size setting as other tools. You can change font sizes and used font for legend, text boxes and tags in “SAS” - “Other tools”-“Concenter common items”.

**Use of the tool**

Test data for this tool are included in the Irena folder distributed in
the zip file and should be in your Igor Pro folder/User procedures/Irena

.. center:: media/PairDistanceDist4.png
      :align: center
      :width: 580px


These data contain Q/Int/error and included is also GNOM generated PDDF in the similarly named file (see center). These can be used to compare the results.

Load data in Igor as seen in the above center and the follow next steps.

Following GUI is available from SAS menu (under Pair Distance dist. Fnct.)

.. center:: media/PairDIstanceDist5.png
      :align: center
      :width: 100%


In this GUI I have already selected the test data and pushed button “Graph”. This created the input graph on the right hand side.

Model Input selection:

PDDF modeling requires few right choices… Here are some suggestions how to get the right values for analysis…

1. Maximum r. Generally this is maximum distance for p(r) (=PDDF) function. For relatively spherical particles it is close to 2\*Rg, for less spherical particles can get larger, may be up to 4\* Rg. It is important to guess large enough number, but not too large. To help, you can try using the button “Guess maximum”. In this case the code will attempt to fit one-level Unified fit to the data and provide guess for Rg. Maximum r is set to 2.5\*Rg. Here is result in this case:

.. center:: media/PairDIstanceDist6.png
      :align: center
      :width: 100%


Note, this fit is not exciting, but the Rg is actually quite good, as you will see later…

2. Next one needs to choose number of bins. Too large number slows down calculations. I am not sure if higher numbers are of much use.

3. Subtract background – if there is some flat background in the data still left, one can subtract it here. Moore’s technique can fit the background. Test data really do not have any background left.

4. Errors handling. There is no perfect selection here. One needs to play and get the right errors handling here. Many SAXS data reduction tools do not produce meaningful errors and each technique required somehow different error handling. “sqrt errors” are meaningful ONLY if the data are still in “counting” statistics. Rare case… However, there are some ideas about the right approach here:

Regularization

Start with higher error multiplier (for User errors of sqrt errors) and then try fitting with decreasing error multiplier. At some point the fit will look good – and when multiplier is decreased even more, the fit will start failing. Lowest multiplier when you can still get fit is probably close to right…

Moore technique

Uses least square fitting. I had better success with using fractional errors. Again, reduce errors to force good with within reasonable number of iterations.

**Regularization**

There is nothing more needed, just select range of data to fit (probably whole range, but can be limited using cursors) and push fit button:

.. center:: media/PairDIstanceDist7.png
      :align: center
      :width: 100%


And here is result… One can see the PDDF, below graph are normalized residuals, provided is Rg and fit int eh graph.

**Moore technique (indirect Fourier Transformation)**

Select the tab with “Moore” and then see below:

.. center:: media/PairDIstanceDist8.png
      :align: center
      :width: 100%


Note, that one has more controls:

“Determine number of functions” – that is useful to make sure reasonable number of function is chosen… I suggest using it, unless you have reason not to.

“Fit background” – if there is flat background left in the data, you can try.

“Fit maximum size” – you can try, but in my experience resulting maximum size seems too low.

**Semi-GNOM file and other output data methods**

There are three buttons to use with three different methods to output data.

From irena version 2.31 is output of Semi-GNOM ASCII file for use in other ATSAS packages. ATSAS is well known package of programs from Dmitri Svergun,  http://www.embl-hamburg.de/ExternalInfo/Research/Sax/software.html . GNOM is program which performs regularization method of PDDF analysis,  same as PDDF in Irena package. Its output file is being used by all other ATSAS programs, such as DAMMIN etc. A user has requested that I provide method of outputting output file compatible with GNOM to use with results from Irena PDDF tool.

The GNOM file format does not seem to be publicly described and therefore, I had to reverse engineer which parts of the GNOM file are actually important for other programs and formatting of all different fields, as the formatting seems to be really unusual and obsolete.

The provided data format has been tested on DAMMIN PC version 5.3 and attempts to follow the GNOM file version 4.4 included as example with DAMMIN. I cannot guarantee any functionality. If you find case when it does not work, send me the Igor experiment and all other related details and I will try tooimprove the compatibility, if I can.

Note, not all parameters printed in the output file are meaningful for Irena PDDF tool. Some of them are there because they just seem to have to be there.

Here is snippet of the GNOM output file, red are my comments

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

#### G N O M --- Version 4.4 #### Header, must be here

Thu Sep 25 08:44:00 2008 Date, meaningful

=== Run No 1 === meaningless

Run title: root:SAS:ImportedData:lyzexp:R\_lyzexp Your data name,
meaningful

\*\*\*\*\*\*\* Input file(s) : R\_lyzexp meaningful

Condition P(rmin) = 0 is used. meaningless

Condition P(rmax) = 0 is used. meaningless

Highest ALPHA is found to be 1 meaningless

#### Final results #### meaningless

Angular range : from 0.0414 to 0.4984 meaningful

Real space range : from 0.00 to 50.00 meaningful

Current ALPHA : 0.10E+01 Rg : 0.153E+02 I(0) : 0.655E+01 Alpha is
meaningless, else is meaningful

Real space range : from 0.00 to 50.00 meaningful

S J EXP ERROR J REG I REG meaningful

0.0000E+01 0.6555E+01 meaningful

0.2299E-02 0.6552E+01

0.4598E-02 0.6544E+01

0.6897E-02 0.6530E+01

0.9197E-02 0.6512E+01

0.1150E-01 0.6488E+01

0.1379E-01 0.6459E+01

0.1609E-01 0.6424E+01

0.1839E-01 0.6385E+01

0.2069E-01 0.6341E+01

0.2299E-01 0.6291E+01

0.2529E-01 0.6237E+01

0.2759E-01 0.6179E+01

0.2989E-01 0.6116E+01

0.3219E-01 0.6048E+01

0.3449E-01 0.5977E+01

0.3679E-01 0.5901E+01

0.3909E-01 0.5822E+01

0.4138E-01 0.5904E+01 0.7150E-01 0.5739E+01 0.5739E+01 meaningful

0.4372E-01 0.5652E+01 0.7020E-01 0.5651E+01 0.5651E+01

0.4605E-01 0.5533E+01 0.6995E-01 0.5560E+01 0.5560E+01

….

Distance distribution function of particle meaningful

R P(R) ERROR meaningful

0.0000E+01 -0.5838E-03 0.5818E-04 meaningful

0.5000E+00 0.6171E-04 0.4782E-04

….

Reciprocal space: Rg = 15.252 , I(0) = 0.6555E+01 meaningful

Real space: Rg = 15.252 +- 0.000-00 I(0) = 0.6555E+01 +- 0.000E+00 meaningful, except for errors.

**Other methods of saving data…**

“Save results” copies wave with results into originating data folder. Copied are both model intensity and Q vector, as well as normalized residual. Also copied is PDDF and associated size wave. All of these waves have wave notes with all parameters and are recognized as results by Plotting tool, Data export tool and other Irena tools.

“Paste to Notebook” copies graph and somehow formatted summary of result into special notebook (created if necessary) for printing and future review.

.. center:: media/PairDIstanceDist9.png
      :align: center
      :width: 100%


You can access this notebook (if exists) from “SAS”-“Other tools”-“Show Results notebook” menu. You can save the notebook as RFT file, which then can be edited in any Word processor.
