.. _irena-pair-distance-dist:
.. _model.pdf:

.. index::
   model; PDF
   model; Pair distance distribution function

Pair distance distribution function
=====================================

Model description
-----------------

This tool calculates the pair distance distribution function (PDDF) as defined
in small-angle scattering theory (see, e.g., Glatter and Kratky, 1982, p. 27,
formula 29):

.. math::

      I(Q)=(\Delta\rho)^2V\int_{0}^{D}4\pi r^2\,dr\,\gamma_0(r)\frac{\sin(Qr)}{Qr}
           =(\Delta\rho)^2V\sum_{0}^{D}4\pi r^2\,\Delta r\,\gamma_0(r)\frac{\sin(Qr)}{Qr}

where (Δρ)²V is the contrast/volume scaling factor. This factor is set to 1 in
this tool — the same convention used by GNOM.

The PDDF is γ\ :sub:`0`(r), where r is the distance in Å.

Two methods are implemented:

* **Regularization** (by Pete Jemian)
* **Moore's method** (Moore, P. B., J. Appl. Cryst. 13, 168–175, 1980)

Both methods have specific advantages and limitations. The tool has been tested
against the widely used GNOM software (D. I. Svergun, EMBL) and produces
consistent results on the available test cases.

From version 2.41, the tool also outputs the gamma function:
γ = PDDF / (4π r²).

.. note::

   Significant changes in the calculated PDDF can occur with changes in the
   assumed maximum dimension or in the error scaling. Observations on this
   sensitivity are noted below.

Graph font and font size follow the common Irena settings, configurable from
SAS → Other tools → Configure common items.

Use of the tool
---------------

Test data are included in the Irena distribution folder
(``…/Igor Pro/User Procedures/Irena``).

.. Figure:: media/PairDistanceDist4.png
   :align: center
   :width: 580px

These data contain Q/Intensity/error, and a GNOM-generated PDDF file is
included for comparison (center panel). Load both into Igor and proceed.

The PDDF GUI is available from SAS → Pair Distance dist. Fnct. The top section
contains :ref:`standard data selection tools <DataSelection>`.

.. Figure:: media/PairDistanceDist5.png
   :align: center
   :width: 100%

Model input selection
~~~~~~~~~~~~~~~~~~~~~

1. **Maximum r** — the maximum particle dimension for the p(r) function.
   For roughly spherical particles, a good starting point is ~2 × R\ :sub:`g`;
   for elongated particles, up to ~4 × R\ :sub:`g`. Use the "*Guess maximum*"
   button to fit a one-level Unified model to the data and set the maximum r
   to 2.5 × R\ :sub:`g` automatically:

   .. Figure:: media/PairDistanceDist6.png
      :align: center
      :width: 100%

   The fit quality is not critical here; the R\ :sub:`g` estimate is generally
   adequate.

2. **Number of bins** — a large number slows calculation; higher values do not
   significantly improve results in most cases.

3. **Subtract background** — if a flat background remains in the data, it can
   be subtracted here. Moore's method can also fit the background as a free
   parameter.

4. **Errors handling** — there is no universally correct approach. The
   appropriate choice depends on the data and the reduction method:

   * *Regularization:* Start with a high error multiplier and reduce it until
     the fit begins to degrade. The lowest multiplier that still produces a
     good fit is approximately correct.
   * *Moore technique:* Fractional errors typically give better results.
     Reduce errors to force convergence within a reasonable number of iterations.

Regularization
~~~~~~~~~~~~~~

Select the data range with cursors and click "*Fit*":

.. Figure:: media/PairDistanceDist7.png
   :align: center
   :width: 100%

Results show the PDDF, normalized residuals, and R\ :sub:`g`.

Moore technique (indirect Fourier transformation)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Select the "Moore" tab:

.. Figure:: media/PairDistanceDist8.png
   :align: center
   :width: 100%

Additional controls:

"*Determine number of functions*" — recommended; ensures a reasonable number
of basis functions is used.

"*Fit background*" — use if a flat background remains in the data.

"*Fit maximum size*" — available but tends to produce an underestimated maximum
dimension.

Semi-GNOM file and other output methods
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Three output buttons are available.

From Irena version 2.31, a semi-GNOM ASCII file can be exported for use in
other ATSAS programs (http://www.embl-hamburg.de/ExternalInfo/Research/Sax/software.html).
GNOM (D. I. Svergun) performs regularization-based PDDF analysis and its output
format is used by ATSAS tools such as DAMMIN. This export option provides
compatible output from the Irena PDDF tool.

The GNOM file format is not publicly documented; the format was reverse-engineered
from examples. The implementation has been tested against DAMMIN PC version 5.3
and targets GNOM output version 4.4. Compatibility cannot be guaranteed in all
cases. If incompatibility is found, contact the developer with the Igor experiment
and details.

Note that not all fields in the GNOM output are meaningful in the Irena context —
some are included because they appear to be required by downstream programs.

Below is an annotated snippet of the GNOM output format::

    #### G N O M --- Version 4.4 ####          Header, required
    Thu Sep 25 08:44:00 2008                   Date, meaningful
    === Run No 1 ===                           meaningless
    Run title: root:SAS:ImportedData:lyzexp:R_lyzexp   data name, meaningful

    ******* Input file(s) : R_lyzexp           meaningful
    Highest ALPHA is found to be 1             meaningless
    #### Final results ####                    meaningless
    Angular range : from 0.0414 to 0.4984      meaningful
    Real space range : from 0.00 to 50.00      meaningful
    Current ALPHA : 0.10E+01  Rg : 0.153E+02  I(0) : 0.655E+01
                               (Alpha meaningless; Rg and I(0) meaningful)
    S  J EXP  ERROR  J REG  I REG             meaningful
    ...

    Distance distribution function of particle  meaningful
    R  P(R)  ERROR                              meaningful
    ...
    Reciprocal space: Rg = 15.252 , I(0) = 0.6555E+01   meaningful
    Real space: Rg = 15.252 +- 0.000E+00 , I(0) = 0.6555E+01  (errors not meaningful)

**Other saving methods:**

"*Save results*" — Copies the model intensity, Q vector, normalized residual,
PDDF, and associated size wave into the originating data folder. Wave notes
contain all parameters. Results are recognized by Irena's Plotting tool, Data
export tool, and other tools.

"*Paste to Notebook*" — Copies the graph and a formatted results summary to the
special results notebook (created if it doesn't already exist).

.. Figure:: media/PairDistanceDist9.png
   :align: center
   :width: 100%

Access the notebook from SAS → Other tools → Show Results notebook. Save as RTF
for use in a word processor.
