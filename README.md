# SPA660: High Energy Astrophysics of Binary Star Systems
## Project 10: X-ray Spectral Analysis of WD 2226−210

This repository holds the data-reduction commands and spectral-fitting setup for Project 10, which looks at the hard X-ray emission from WD 2226−210, the hot DAO central star of the Helix Nebula. We reprocess a single Chandra ACIS-S observation and check whether we can recover the spectral results published by Estrada-Dorado et al. (2025). The full discussion is in the final project report.

### Overview

* **Target:** WD 2226−210, a DAO white dwarf with T_eff of about 0.95–1.1 × 10^5 K
* **Data:** Chandra ACIS-S, ObsID 631 (36.7 ks raw exposure, 30.55 ks after flare screening)
* **Question:** Hard X-rays above 0.5 keV cannot come from a photosphere this cool. Can we reproduce the published spectral parameters, and what do they tell us about accretion as the source of the emission?

### Main results (0.3–2.0 keV)

We fitted an absorbed single-temperature thermal plasma (`tbabs×apec`) with the `angr` abundance table. Every quantity below agrees with the published value to within 1σ.

| Quantity | Our value | Comment |
| :--- | :--- | :--- |
| N_H | (1.6 ± 1.1) × 10^20 cm^−2 | Consistent with the Galactic column in this direction. We see no sign of absorption local to the star, but the data cannot tell zero from about 3.7 × 10^20 cm^−2. |
| kT | 0.896 ± 0.014 keV | Corresponds to T = 1.04 × 10^7 K, about a hundred times the photospheric temperature. This is the most robust parameter in our sensitivity tests. |
| Unabsorbed flux | (1.20 ± 0.06) × 10^−13 erg s^−1 cm^−2 | Shifts by about 7% depending on the fit statistic. |
| L_X (d = 200 pc) | (5.75 ± 0.30) × 10^29 erg s^−1 | Scales as d^2. |
| Accretion rate | about (1–2) × 10^−10 M_⊙ yr^−1 | From the relation in the reference paper, for M_WD = 0.6–0.65 M_⊙. |

### Caveats

The single-temperature fit is not a good one (χ²/dof = 85.97/49), and the residuals are structured, mostly between 0.7 and 1.3 keV. We therefore read kT as an effective temperature, not that of an isothermal gas. The normalisation and luminosity also depend on the abundance table, and the accretion rate rests on an assumed white-dwarf mass. Accretion is a plausible explanation for the emission, but our data do not prove it. A simple energy-budget estimate leaves the available accretion power about three orders of magnitude above the observed L_X, and we could not resolve that gap.

### Software

* CIAO 4.18.0 (CALDB 4.12.4)
* HEASoft 6.37.1, with XSPEC 13.0.1
* AtomDB 3.1.3

### Repository contents

* `631/primary/`: the unreduced observation data and aspect solutions for ObsID 631
* `pipeline_commands.md`: the annotated log of every command we ran, covering flare screening (5–10 keV light curve, threshold 0.78 counts s^−1), source and background extraction (5″ circle, 8″–20″ annulus) and the XSPEC fits
* `.gitignore`: keeps local cache and parameter files out of the repository

---
**Indian Institute of Technology Kanpur**  
*SPA660 - High Energy Astrophysics of Binary Star Systems*
*Members: Kushagra Rajput, Shriom Gupta, Prachi Jain*  
*Instructor: Prof. Kunal Mooley (November 2026)*
