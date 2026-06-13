# Gabor-Deconvolution-and-DTW-for-Automatic-Well-Seismic-Tie

> Automatic well-seismic tie framework using Gabor Deconvolution for time-variant wavelet estimation and Dynamic Time Warping for synthetic-to-seismic alignment | Torosa Field, Browse Basin

---

## Overview

Automatic well-seismic tie on Q-attenuated data faces two fundamental challenges:

1. **Wavelet non-stationarity** — seismic wavelets change with depth due to frequency decay from Q attenuation
2. **Temporal misalignment** — synthetic seismograms are rarely aligned with real seismic traces without manual intervention

This repository presents an integrated pipeline that addresses both challenges automatically:

- **Gabor Deconvolution** estimates time-variant wavelets in the time-frequency domain, recovering wavelet frequency and amplitude components degraded by Q attenuation
- **Fixed Band DTW** serves as a comparative approach, applying DTW within a fixed search band without geological anchor constraints
- **Marker-Anchored DTW** aligns synthetic seismograms to seismic traces using formation marker positions as hard-constraint anchor points, preventing geologically unreasonable stretching
  
The method was validated on controlled synthetic data and applied to five wells in the **Torosa Field, Browse Basin**.

---

## Key Results

### Synthetic Validation — Gabor Deconvolution
| Metric | Value |
|---|---|
| Global correlation (r) | 0.951 |
| Frequency RMSE | 7.96 Hz |

> Phase estimation at deeper zones was not fully achieved due to Hilbert transform instability as SNR decreases with cumulative Q attenuation.

### Synthetic Validation — Marker-Anchored DTW vs Fixed Band DTW
| Method | Correlation (r) | Tops Error |
|---|---|---|
| Marker-Anchored DTW | 0.990 – 0.993 | **0 markers** across all ±10%–±30% scenarios |
| Fixed Band DTW | Comparable | **4–6 markers** per scenario |

### Field Data (Torosa Field, Browse Basin)
| Condition | Correlation (r) | Tops Error |
|---|---|---|
| Pre-tie | -0.236 to 0.205 | — |
| Post-tie (Marker-Anchored DTW) | **0.571 to 0.800** | **0 across all 5 wells** |

Wells: NSR-1, Torosa-2, Torosa-3, Torosa-4, Torosa-5

> On the 2D seismic section, all markers from the Barracouta Formation to the Triassic are laterally consistent on the same reflector across all five wells.

---

## Repository Structure

```
.
├── data/               # Input data (LAS logs, SEG-Y seismic, checkshot)
├── notebooks/          # Jupyter notebooks for each stage of the pipeline
└── README.md
```

---

## Pipeline

```
LAS Log + Checkshot
        │
        ▼
 Synthetic Seismogram
        │
        ▼
 Gabor Deconvolution  ──►  Time-Variant Wavelet Estimation
        │
        ▼
 DTW Alignment  ──►  Marker-Anchored DTW  (proposed)
                ──►  Fixed Band DTW       (baseline)
        │
        ▼
   Well-Seismic Tie
```

---

## Methods

### Gabor Deconvolution
Gabor Deconvolution operates in the time-frequency domain using the Short-Time Fourier Transform (STFT), enabling estimation of wavelets that vary with time (depth). This is particularly important for data with significant Q attenuation, where a stationary wavelet assumption breaks down. The method recovers wavelet frequency and amplitude components that have been degraded by cumulative Q attenuation with depth.

### Fixed Band DTW
Fixed Band DTW applies Dynamic Time Warping within a predefined search band of fixed width, without any geological anchor constraints. While this approach can achieve comparable correlation values, it does not guarantee that formation markers remain correctly positioned after alignment, leading to tops errors of 4–6 markers per scenario in synthetic tests.

### Marker-Anchored DTW
Dynamic Time Warping (DTW) finds the optimal warping path between a synthetic seismogram and a seismic trace. The Marker-Anchored variant constrains the warping path at known formation marker positions, ensuring that geological boundaries remain pinned during alignment and preventing stratigraphically unreasonable stretching or squeezing. Marker positions act as hard constraints, making the alignment geologically consistent across wells.

---

## Requirements

```bash
pip install numpy scipy matplotlib pandas segyio lasio jupyter
```

---

## Citation

If you use this work, please cite:

```
Rizaldi, F. S. (2026). Automatic Approaches to Well-Seismic Tie: Integrating 
Gabor Deconvolution-Based Wavelet Extraction and Dynamic Time Warping at Torosa 
Field, Browse Basin. Final Project, Department of Geophysical Engineering, 
Institut Teknologi Sepuluh Nopember (ITS), Surabaya.
```

---

## Author

**Firza Rizaldi Suryantoro**  
Geophysical Engineering, Institut Teknologi Sepuluh Nopember (ITS)  
Supervised by Ir. Firman Syaifuddin, S.Si., M.T. and Nilla Perdana Agustina, S.T., M.T.
