# bbCARMEN Automated Image Analysis Pipeline

This repository contains code and workflows for automated image analysis and downstream data processing of **bbCARMEN** assays. The pipeline leverages **CellProfiler 3.5** and a custom **Jupyter notebook** to identify, track, and classify beads in multiplexed viral detection experiments.

## Overview

The bbCARMEN analysis pipeline performs:

- Bead detection and filtering using **CellProfiler**
- Color channel correction and droplet masking
- Timepoint-based bead tracking
- Bead clustering and virus assignment via **k-means**
- FAM fluorescence quantification and classification
- Visualization and QC reporting

## Workflow Summary

### 1. Bead Detection & Filtering (CellProfiler)

- Illumination correction by background subtraction (per channel)
- Color bleedthrough correction via image subtraction
- Droplet masking to exclude well edge artifacts
- Bead filtering:
  - By shape: solidity, eccentricity
  - By proximity: exclusion of beads near neighbors
  - By droplet content: exclusion of multi-bead droplets

### 2. Bead Feature Extraction

- Each accepted bead’s mask is expanded by 5 pixels to define a **donut region**
- FAM (blue channel) intensity is measured in this donut region

### 3. Bead Tracking Across Time

- Uses **Linear Assignment Problem (LAP)** framework to track beads across timepoints

### 4. Bead Classification (Jupyter Notebook)

- Normalized RGB features are computed per bead:
  - Normalized = channel intensity / sum of RGB intensities
- K-means clustering on normalized red, green, and yellow values
- Ternary plot visualization with cluster coloring
- Each cluster matched to a known virus based on proximity to predefined centroids

### 5. FAM Fluorescence Quantification

- Median blue intensity in the donut region used to classify virus presence per sample
- Classification thresholding based on:
  - Fold-difference from within-well negative controls
  - or number of standard deviations above the negative control median
- FAM fluorescence kinetics visualized over time

## Outputs

- Ternary cluster plots for bead classification
- Virus detection calls per sample
- Kinetic plots of FAM fluorescence
- QC plots and metrics

## Code Repository

👉 [https://github.com/broadinstitute/bbCARMEN_analysis](https://github.com/broadinstitute/bbCARMEN_analysis)

## Authors

Tien G. Nguyen  
Rebecca Senft  
David R. Stirling  
Sameed M. Siddiqui  
Nicole L. Welch  
Cheri M. Ackerman  
Paul C. Blainey  
Pardis C. Sabeti  
Cameron Myhrvold

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
