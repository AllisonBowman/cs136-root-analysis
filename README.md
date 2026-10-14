# 136Cs ROOT Analysis Toolkit

This repository contains a set of ROOT-based C++ functions developed for the analysis of neutron time-of-flight (TOF) spectra from experiments conducted at the Triangle Universities Nuclear Laboratory (TUNL) investigating decay sequences of **Cesium-136 (136Cs)**.

## Project Overview

The primary goal of this project was to analyze β-decay transitions and neutron emission following Gamow-Teller transitions in 136Cs. The experimental data was collected using a gaseous xenon target bombarded with a 6.8 MeV proton beam, resulting in the formation of excited 136Cs nuclei. The TOF spectra from neutron and γ-ray detectors were analyzed to identify specific excited nuclear states and to search for potential missing transitions.

## Key Features

- **TOF Histogram Construction**: Code organizes raw ROOT files into `TH1F` objects by detector cluster (top, mid, bottom) and angle (0° to 12°).
- **Detector Synchronization**: Time alignment of signals using γ-ray peaks as a timing reference.
- **Polynomial Background Subtraction**: Allows subtraction of background using customizable polynomial models.
- **Peak Fitting**:
  - Single Gaussian and triple Gaussian peak fitting functions.
  - Fit centers anchored to theoretical TOF positions for known 136Cs states from literature.
  - Convolution with gamma-ray response shape using a parametrized triple-Gaussian model.
- **Compound Nuclear Modeling**: Includes simulated neutron smearing from TRIM modeling and non-selective decay models to explain angular independence.
- **Angular Comparison**: Enables comparison of transition intensities across angles to test for preferential state selection.

## Repository Contents

- `tunl_pn_2dplot_all_detectorsAPB.C` — builds the combined neutron time-of-flight histogram from the per-detector ROOT files, applying the per-channel timing offsets that align the detectors (detector synchronization), and fits peaks with a polynomial background and single/double-Gaussian signal (`twodplot`, `fit`, `peakFIND`).
- `tunl_pn_2dplot_APBgammafit.C` — the same pipeline extended with the γ-ray response model: each neutron peak is fitted as a triple Gaussian whose widths, amplitudes and spacings come from the γ-ray peak fit, with fit centres anchored to the known 136Cs state energies (`twodplotalldetectors`, `fit`, `fitgamma`).
- Fitting functions are tied to theoretical state energies taken from:
  - TUNL 136Cs state evaluations
  - Rebeiro Ph.D. thesis (University of the Western Cape)
  - NNDC and AME2016 atomic mass evaluation data
- The TRIM-smearing / compound-nucleus fitting script described under Key Features is not in this repository.

## How to Use

1. **Install ROOT** (root.cern).
2. **Load a macro and call its entry function** from the ROOT prompt:
   ```bash
   root -l
   .L tunl_pn_2dplot_all_detectorsAPB.C
   twodplot(0, 1, 2, ...)      // detector indices to combine
   fit(l, h)                   // fit the channel range [l, h]
   ```
   or, for the γ-response model, `.L tunl_pn_2dplot_APBgammafit.C`, then `twodplotalldetectors(...)`, `fit(l, h)` and `fitgamma(l, h)`.

## Notable Parameters and Models

- **Gaussian Fit Model**:
  ```cpp
  f(x) = A * exp(-0.5 * ((x - (μ + δμ)) / σ)^2)
  ```
  - Amplitude (A), width (σ), and offset (δμ) are optimized by the fitting routine.
- **Triple Gaussian Response Model**: Matches the γ-ray signal structure to model neutron peaks more accurately.
- **Smearing Function from TRIM**:
  ```math
  σ = a*μ³ - b*μ² + c*μ - d
  ```

## Findings

- **No angular dependence** was observed in the neutron spectrum, supporting the hypothesis of compound nuclear decay over selective transition rules.
- **Several previously unreported or unresolved states** were indicated in the neutron TOF fits, suggesting further exploration in the >2.6 MeV energy region may be fruitful.

## Citation

Please cite this code and project using:

> Bowman, A. Daniels, T PhD. (2023). *Analyzing Nuclear Decay Sequence of Cesium*. Triangle Universities Nuclear Laboratory.

## Acknowledgements

This project was developed under the mentorship of Dr. Daniels at TUNL. Support and data courtesy of the TUNL collaboration.
