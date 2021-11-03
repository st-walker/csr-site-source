---
title: "Gpt Csrtrack Ocelot"
date: 2021-09-21T10:33:58+02:00
draft: true
---

# Overview

Here I am going to compare CSRTRack, OCELOT and GPT with application to CSR simulations
in bunch compressors.


# The Zeuthen Benchmark Chicane

  The [Zeuthen chicane](https://www.desy.de/csr/) is the standard chicane used in the
  literature when evaluting different CSR codes, and is accordingly used here.  The
  chicane and beam parameters parameters are below.  The chicane parameters are a subset
  of the ones on the website

  The beam parmaters (with compression) are the same as the ones outlined on the website
  except with a reduced energy from 5GeV to 0.5GeV, motivated by a desire to possibly
  demonstrate any non-radiative CSR effects.  Furthermore, the simulation excludes any
  longitudinal charge density modulation, as well as the uncorrelated energy spread
  being set to 10keV.  The phase space for this input distribution is shown as well.


<table>
<tr><th>Zeuthen chicane parameters </th><th>Zeuthen beam parameters</th></tr>
<tr><td>


  | Parameter                                  | Value | Unit   |
  |--------------------------------------------|-------|--------|
  | Projected drift length 1/2, 3/4            |   5.0 | m      |
  | Drift length 2/3                           |   1.0 | m      |
  | Dipole length                              |   0.5 | m      |
  | Post chicane drift                         |  2.0  | m      |
  | Total projected length                     |  13.0 | m      |    
  | R<sub>56</sub>                             | -25   | mm     |
  | T<sub>566</sub>                            | +37.5 | mm     |  
  | Bending angle                              | 2.77  | &#176; |
  | Bending radius                             | 10.35 | m      |
  | Vertical halfgap of dipoles                | 5     | mm     |

</td><td>

  | Parameter                                  | Value    | Unit           |
  |--------------------------------------------|----------|----------------|
  | Beam energy                                |    0.5   | GeV            |
  | Linear energy-z correlation                |   36     | m<sup>-1</sup> |
  | Normalised emittance                       |    1.0   | mm&middot;mrad |
  | Bunch length                               |  200     | &mu;m          |
  | Peak current                               |    0.6   | kA             |
  | Bunch charge                               |    1     | nC             |
  | Uncorrelated energy spread                 |   10     | keV            |
  | &beta;<sub>x,y</sub>                       |   40, 13 | m              |
  | &alpha;<sub>x,y</sub>                      | 2.6, 1.0 | m              |  

</td></tr> </table>


![input-distro](/gpt/zeuthen-primary.pdf "Input beam distribution for nominal Zeuthen")

The increase in the slice energy spread at the tails of the distribution is wsimply an
artefact of the algorithm used to calculate it.  The algorithm proceeds by accumulating
N macroparticles per slice.  At the tails the number of macroparticles is small compared
to N, so the slice is relatively long in z, resulting in the slice energy spread being
dominated by the linear chirp, and rises accordingly..  This is a common feature in
plots that follow. Lastly the distributions are started 10cm upstream of the first
dipole entrance to ensure no spurious dispersion from the bunch overlapping with the fringe
fields.

# Optical tracking without collective effects

  This is necessary as a sanity check before further more time-consuming simulations.
  CSRTrack, OCELOT and GPT for the Zeuthen benchmark are all run without any collective
  effects.


# Input distributions

  The distribution for the Zeuthen benchmark is shown bel
  

## CSR use case 1

##

