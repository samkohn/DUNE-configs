Flux Configuration
=======

### For the DUNE Fast Monte Carlo

--------

#### Description

#### Beam flux files

The .csv files give the beam flux used in the Fast Monte Carlo
calculations. The filename is split into three parts: (a)nuflux, which
describes whether the beam is in neutrino or antineutrino mode; nu(e,
mu, tau)(bar)flux, which describes the particular neutrino being
produced; and a number describing the number of energy bins between
0 and 10 GeV (e.g. 40). The units for the flux are: neutrinos/0.125
GeV/m^2/POT, where the 0.125 GeV is the bin width. The files in the
CD1-CDR-FHC folder are derived from the exact flux used for the DUNE
CDR [arXiv: 1512.06148] neutrino mode/forward horn current at 120 GeV,
which, according to the CDR volume 3 [arXiv 1601.05823], corresponds to
1.1e21 POT/year and 1.2 MW.

#### Flux optimization procedure

The flux [^1] we use starts with the flux histograms produced by the
flux group. We have made a few modifications to these in order to solve
a couple problems with how they are consumed by GENIE. GENIE takes a
flux histogram, randomly chooses a bin treating the histogram as a PDF
(which is okay), then randomly chooses an energy from within the bin
according to a uniform distribution (which is a problem). Also it is not
able to handle varying width bins. Also the flux group provides a flux
with significant statistical fluctuations, especially in the high energy
tail.

We have a piece of code that takes the histogram, smooths the lower
statistics defocused and nue components a little bit, then does a spline
interpolation to produce a finer binned histogram for input to GENIE.
The smoothing isn't perfect, so the spline interpolation in some places
artificially follows the input fluctuations. For the high energy tail
where the fluctuations are too large for smoothing to help, we fit to an
exponential and produce entries in the finer binned histogram from that.

Of course, if we pick randomly from this new histogram with the original
binning, we should obtain a very good approximation to the original
input histogram. We have checked this and find a distortion of 1% or
less from 1.0 GeV up, but distortions of 2% to 4% below 1 GeV, and a
major distortion of 10s of percent for events with less than 0.1 GeV
energy. The sources of this distortion are understood: the non-uniform
slope of the distribution means we should set up the interpolation not
exactly at the simple bin center; the treatment of the interpolation
near the origin should be improved; and the statistics of the input
histogram can be improved. At this point, we think we have a small
enough systematic distortion to continue.

[^1]: Adapted from the FastMC [wiki](https://cdcvs.fnal.gov/redmine/projects/fast_mc/wiki/Physics_and_Detector_Inputs_and_Assumptions#The-flux "").
