# SPA660: High Energy Astrophysics of Binary Star Systems
## Project 10: X-ray Spectral Analysis of WD 2226−210

This repository hosts the data reduction pipelines, calibration structures, and spectral fitting configurations for **Project 10**, focusing on the hot DAO central star of the Helix Nebula (**WD 2226−210**). The analysis successfully reproduces and evaluates the hard X-ray parameter constraints described by Estrada-Dorado et al. (2025).

### Project Overview
* **Target:** WD 2226−210 (DAO White Dwarf, $T_{\text{eff}} \approx 0.95\text{--}1.1 \times 10^5\text{ K}$)
* **Dataset:** Chandra ACIS-S Archive (`ObsID 631`, 36.7 ks raw baseline)
* **Objective:** Address the hard X-ray emission ($>0.5\text{ keV}$) originating from an extremely hot white dwarf core by testing a planetary debris accretion model framework.

### Key Replicated Parameters (0.3–2.0 keV Band)
Using a fully absorbed optically thin thermal plasma model framework (`tbabs × apec`) under `angr` cosmic abundances, the local pipeline extracted the following robust parameter values (matching within $1\sigma$ variance levels of the primary literature reference):

| Physical Parameter | Value | Notes / Significance |
| :--- | :--- | :--- |
| **Neutral Hydrogen Column ($N_{\text{H}}$)** | $(1.6 \pm 1.1) \times 10^{20}\text{ cm}^{-2}$ | Fully consistent with standard Galactic line-of-sight foregrounds; no heavy local circumstellar absorption detected. |
| **Plasma Temperature ($kT$)** | $0.896 \pm 0.014\text{ keV}$ | Corresponds to a shock layer temperature of $T \approx 1.04 \times 10^7\text{ K}$ ($\sim 100\times$ hotter than the stellar surface). |
| **Unabsorbed X-ray Flux ($f_{\text{X}}$)** | $(1.20 \pm 0.06) \times 10^{-13}\text{ erg s}^{-1}\text{cm}^{-2}$ | Evaluates overall radiative balance across the 0.3–2.0 keV window. |
| **X-ray Luminosity ($L_{\text{X}}$)** | $(5.75 \pm 0.30) \times 10^{29}\text{ erg s}^{-1}$ | Derived assuming a baseline distance metrics of 200 pc. |
| **Implied Accretion Inflow ($\dot{M}_{\text{acc}}$)** | $\approx (1\text{--}2) \times 10^{-10}\text{ }M_{\odot}\text{ yr}^{-1}$ | Evaluated across an assumed white dwarf mass footprint of $0.6\text{--}0.65\text{ }M_{\odot}$. |

###Software Environment Setup
Data processing requires a synchronized Linux architecture hosting the following deployment layers:
* **CIAO Tools Framework:** v4.18 (CALDB v4.12.4 calibration files map library)
* **HEASoft Tools Architecture:** v6.37.1 (incorporating explicit `XSPEC v13.0.1` instances)
* **Atomic Modeling Baseline:** `AtomDB v3.1.3` (providing fundamental emission lines structures)

### Repository Structure
* `/631/primary/` : Direct unreduced observation level event datasets and aspect solutions.
* `pipeline_commands.md` : Comprehensive annotated command log covering CIAO flare screening filters (`counts < 0.78 s^-1`), source extraction coordinates ($5''$ circle apertures), and scriptable interactive XSPEC fitting steps.
* `.gitignore` : Keeps repository configurations isolated from local parameter cache rewrites.

---
**Indian Institute of Technology Kanpur**  
*SPA660 Department of High Energy Astrophysics Project Group 10*  
*Members: Kushagra Rajput, Shriom Gupta, Prachi Jain*  
*Instructor: Prof. Kunal Mooley (November 2026)*
