---
title: "The eSASE chicane and CSR"
date: 2021-09-16T13:51:55+02:00
draft: false
---

# Overview

A set of benchmarks for the study of the proposed eSASE chicane in the EUXFEL.  It is
important to understand how the beam may be degrade due CSR in the chicane, and whether
or not the optical configuration by introducing &beta;-function waist in the fourth
dipole. If CSR degradation in the chicane even with the existing FODO optics is
tolerable then all is well.

The parameters of the chicane and the input beam distribution are as follows:


  | Parameter                                  | Value | Unit   |
  |--------------------------------------------|-------|--------|
  | Projected drift length 1/2, 3/4            |     1 | m      |
  | Drift length 2/3                           |   0.4 | m      |
  | Dipole length                              |   0.6 | m      |
  | R<sub>56</sub>                                       | -190.5 | µm     |
  | Bending angle                              | 0.47   | &#176;       |
  | Bending radius                             | 72.72 | m |
  | | | |
  | Beam energy                                |    14 | GeV    |
  | Linear energy-z correlation (central peak) |  5455 | m<sup>-1</sup> |
  | Average &beta;<sub>x,y</sub>                          |    32 | m      |
  | Initial normalised emittance                          |   0.6 | µm     |
  | Initial peak current                       |     5 | kA     |
  | Bunch charge                               |   250 | pC     |
  | Uncorrelated energy spread                 |   2.5 | MeV    |


## Initial distribution

As stated above the intention is to study two sets of optics, standard FODO optics and
optimised optics.  The FODO optics are shown below.  All simulations below are run using
these exact optics.  I have yet to do a more optimal one.

![fodo-optics](/esase/optics-sub-optimal.pdf "FODO optics in the eSASE chicane")

The projected slice lengths for this configuration are below, and it can be seen that
the projected slice lengths are small compared with the bunch length above (~20µm).  The
projected lengths were derived using the equation stated in [this
paper](https://www.desy.de/fel-beam/data/talks/papers/impact_of_optics_2005.pdf).  The
waist s<sub>w</sub> is equal to 0 in this configuration, leading to a relation that is
dominated by the R<sub>51</sub> due to the small angle (R<sub>56</sub>), which is
equivalent in magnitude on both sides of the chicane.  The acumulated R<sub>51</sub> and
R<sub>52</sub> through the chicane are shown.  However the lengths are comparable in
length to that of the final eSASE spike (100nm).

![projected-slice-length](/esase/sigma_proj-r51-r52.pdf)

The input distribution (without energy modulation) is as follows:

![esase-primary](/esase/esase-primary.pdf "Input distribution without chirp")

The energy modulation with the longitudinal beam profile
overlaid is shown below.  The width of the central peak's linear part is about 0.2µm,
giving a linear chirp of about 5455m<sup>-1</sup>.  For maximal compression, this corresponds to
an R<sub>56</sub> of -1/5455m<sup>-1</sup> = -190.5µm.

![esase-emod](/esase/esase-modulation-with-beam.png "Energy modulation")

# Tracking without any CSR

First it is useful to examine the compression without any collective effects to ensure
that the beam and chicane are implemented correctly.  Shown below are the beam
distributions before and after the chicane.  The chicane and the chirp bring the peak
current to nearly 20kA, as expected.

## OCELOT tracking without CSR

![esase-no-csr-ocelot](/esase/ocelot_compared_beams_no_csr.png "OCELOT before and after chicane, no CSR")

# Tracking with CSR but without energy modulation

To simply study the impact of CSR on a bunch of this length in the given chicane, I
simulated without any compression.  There is a slight deformation in the bunch slice
positions shown in the top right, but no growth in the slice or projected emittances at
all.

## GPT with CSR, no energy modulation
![esase-no-csr-gpt](/esase/gpt-output-csr-without-chirp.pdf)

## CSRTrack with CSR, no energy modulation
![esase-no-csr-gpt](/esase/csrtrack-output-csr-without-chirp.pdf)




# Tracking with CSR

As the chicane and input beam distributions seem to be working correctly, now to include
CSR effects.

### Tuning simulation parameters

In GPT I set the `accuracy` (number of decimal places in &gamma;&beta;) to 6, which is
the same as used for the Zeuthen benchmark.

In OCELOT there are a number that must be adjusted to ensure physical results.  The two
most pertinent are `unit_step` of the `Navigator` class and `sigma_min` of the `CSR`
class.  As I understand it, `unit_step` must be set sufficiently small to ensure that
the Navigator doesn't take very large steps (otherwise the backtracking employed when
calculating the CSR wake will be wrong, as it uses the rigid beam approximation I
think), and `sigma_min` must sufficiently small to not smooth out any small details (but
not so small that one will see artificial microbunching).  The central spike goes down
to about 100nm in length, so I chose a value of `sigma_min` of 10nm (one tenth the beam
size or the length of some feature of interest is the rule of thumb I used).  The
`unit_step` I set to 1 cm on the basis of convergence studies I performed for the
Zeuthen benchmark, but obviously this is a different chicane so this could indeed be
wrong here.

For CSRTrack (1D) I used a `sigma_long` of 10nm, using the same reasoning
as for `sigma_min` in OCELOT.  The rest of the CSRTrack tracking configuration:

```
  track_step{precondition=yes
             iterative=2
             error_per_ct=0.001
             error_weight_momentum=0.0

             ct_step_min=0.02
             ct_step_max=0.10
             ct_step_first=0.10
             increase_factor=1.5
             arc_factor=0.3
             duty_steps=yes
            }
```


## OCELOT CSR

No impact on the slice or projected emittances can be seen.  The different post-chicane peak current
shown here to OCELOT tracking without CSR is generally because the peak current is
sensitive to the initial seed for some reason.

![ocelot-esase-with-csr](/esase/ocelot-before-after-with-csr.png "OCELOT before and after CSR")


## GPT CSR

![gpt-esase-with-csr](/esase/gpt-output.pdf "GPT before and after CSR")

## CSRTrack CSR

![csrtrack-esase-with-csr](/esase/CSRtrack-output.pdf "CSRTrack befor and after CSR")

# Why CSR doesn't matter for eSASE

The angle for the eSASE chicane is very small, much smaller than in the other
compressing chicanes at the EUXFEL.  The investivated R<sub>56</sub> values of -200µm
and -600µm correspond to dipole angles of 8.84mrad and 14.6mrad, compared with
~(40-50)mrad for BCs 1 and 2.  The figure shows the relationship between the final
projected emittance and the dipole angle.  Clearly in the range of angles for eSASE,
there is no impact, which is not the case for a reasonable BC1 angle.

![ocelot-esase-angle-scan](/esase/angle-vs-projected-emittance.png)


However this does not explain why CSR in the chicane does not impact the emittance.
Given the very strong current spikes after the eSASE chicane, one would typically expect
a strong CSR wake due to the CSR wake integrand's proportionality to the longitudinal
charge density gradient.  Sufficiently small angles do not allow for radiation emitted
by the tail to catch up with the head of the bunch.  The overtaking angles for a range
of bunch lengths in a 0.6m dipole (i.e. the one used in eSASE) is shown below.  It is
clear then at the the 6.3µm bunch length (5kA) that any radiation emitted by the tail
will not catch up with the head in the dipole.  

![overtaking-length](/esase/overtaking-length.png)

This explains why eSASE is not impacted by CSR.  The angle is too small and the dipole
is too short for the radiation to overtake the bunch.  It is possible that transient
effects due to the velocity term will impact the bunch, but these are also suppressed
due to the high energy and low bending radius.  Finally 

![ocelot-esase-max-compression-animation](/esase/esase-200-um-max-compression.gif)

In contrast, for 0.05 radians (comparable to BC1/2), but also featuring overcompression:

![ocelot-esase-max-compression-animation](/esase/bigger-angle-compression.gif)

! Conclusion

CSR can be seen not to have a sizeable impact for the eSASE chicane.  This is ultimately
due to the small angle of the chicane bends.  Firstly, this small angle leads to both a
relatively long overtaking length (beyond the length of a single dipole) so that the
steady-state CSR regime is never reached (i.e. mostly only transient effects).  Secondly
the small angle gives a small dispersion in the chicane, so that any induced wake along
the length of the bunch is only marginally translated to a shift in the slice positions
along the bunch.  Therefore the projected emittance does not change.  The dispersion
effect likely dominates over the CSR one as one can see above that even close to the
overtaking length that the projected emittance does not change.