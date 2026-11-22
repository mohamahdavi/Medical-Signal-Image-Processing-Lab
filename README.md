# Medical Signal and Image Processing Lab

Implementations, datasets, helper utilities and technical reports for the **Medical Signal and Image Processing Laboratory** at **Sharif University of Technology** (Fall 2026).

- **Authors:** Mohammad Reza Mahdavi, Reza Kazemeini Moghaddam
- **Instructor:** Marzieh Barakat
- **Supervisor:** Mohammad Bagher Shamsollahi

The laboratory covers time-frequency analysis of electrophysiological signals and blind source separation (BSS). It contrasts second-order decomposition (SVD/PCA) with fourth-order independent component analysis (`CoM2` and `JADE`) for artifact removal and non-invasive fetal ECG extraction. Every experiment comes with a full technical report in English, with its LaTeX source and figures.

## Contents

- [Experiments at a glance](#experiments-at-a-glance)
- [Repository structure](#repository-structure)
- [1. Biosignal time-frequency analysis](#1-biosignal-time-frequency-analysis)
- [2. EEG denoising with ICA](#2-eeg-denoising-with-independent-component-analysis-ica)
- [3. Maternal-fetal ECG separation (SVD vs. ICA)](#3-maternal-fetal-ecg-separation-svd-vs-ica)
- [Reports](#reports)
- [Prerequisites and usage](#prerequisites-and-usage)
- [Data and course materials](#data-and-course-materials)
- [License](#license)

---

## Experiments at a glance

| # | Experiment | Signals | Main techniques | Headline result | Report |
|---|------------|---------|-----------------|-----------------|--------|
| 1 | [Biosignal Time-Frequency Analysis](<Medical Signal and Image Processing Lab/Biosignal-Time-Frequency-Analysis>) | EEG, ECG, EOG, EMG | DFT, Welch PSD, STFT, anti-aliased downsampling | A 256 → 128 Hz reduction keeps r = 0.986 with the raw samples; 1963 ventricular beats labeled in MIT-BIH record 201 | [PDF](<Medical Signal and Image Processing Lab/Biosignal-Time-Frequency-Analysis/report/Report_EN.pdf>) |
| 2 | [EEG Denoising with ICA](<Medical Signal and Image Processing Lab/EEG-Denoising-with-ICA>) | Simulated and real EEG | CoM2 ICA, component selection, RRMSE | RRMSE 1.778 → 0.715 at −5 dB and 5.623 → 2.084 at −15 dB | [PDF](<Medical Signal and Image Processing Lab/EEG-Denoising-with-ICA/report/Report_EN.pdf>) |
| 3 | [Maternal-Fetal ECG Separation](<Medical Signal and Image Processing Lab/Maternal-Fetal-ECG-Separation-SVD-vs-ICA>) | Simulated abdominal ECG | SVD/PCA vs. JADE ICA | Fetal ECG correlation 0.675 (SVD) vs. 0.998 (ICA) | [PDF](<Medical Signal and Image Processing Lab/Maternal-Fetal-ECG-Separation-SVD-vs-ICA/report/Report_EN.pdf>) |
| 4 | [EEG Patterns: ERP, SSVEP, ERD/ERS](<Medical Signal and Image Processing Lab/EEG-Patterns-ERP-SSVEP-ERD-ERS>) | EEG | P300 averaging, SSVEP spectra, band power | Task description only; report to follow | n/a |

---

## Repository structure

```text
Medical-Signal-Image-Processing-Lab/
├── README.md
├── LICENSE
├── .gitignore
└── Medical Signal and Image Processing Lab/
    ├── Biosignal-Time-Frequency-Analysis/
    │   ├── TASK.md                           task description (English translation)
    │   ├── data/
    │   │   ├── ANNOTATIONS.pdf
    │   │   ├── ECG_sig.mat
    │   │   ├── EEG_fig.png
    │   │   ├── EEG_sig.mat
    │   │   ├── EMG_sig.mat
    │   │   ├── EOG_sig.mat
    │   │   ├── MIT-BIH Arrhythmia_Introduction.pdf
    │   │   └── disp_eeg.m
    │   └── report/
    │       ├── Report_EN.pdf
    │       └── latex/                        LaTeX source and figures
    │
    ├── EEG-Denoising-with-ICA/
    │   ├── TASK.md
    │   ├── data/
    │   │   ├── COM2R.m
    │   │   ├── EMGsorsep.m
    │   │   ├── Electrodes.mat
    │   │   ├── NewData1.mat ... NewData4.mat
    │   │   ├── X_noise.mat
    │   │   ├── X_org.mat
    │   │   ├── disp_eeg.m
    │   │   ├── plotEEG.m
    │   │   ├── plottopomap.m
    │   │   └── tfuni4.m
    │   └── report/
    │       ├── Report_EN.pdf
    │       └── latex/
    │
    ├── Maternal-Fetal-ECG-Separation-SVD-vs-ICA/
    │   ├── TASK.md
    │   ├── data/
    │   │   ├── A.dat, X.dat, Z.dat, observation.dat
    │   │   ├── mecg1.dat, mecg2.dat
    │   │   ├── fecg1.dat, fecg2.dat
    │   │   ├── noise1.dat, noise2.dat
    │   │   ├── ica.m, jade.m
    │   │   └── plot3ch.m, plot3dv.m
    │   └── report/
    │       ├── Report_EN.pdf
    │       └── latex/
    │
    └── EEG-Patterns-ERP-SSVEP-ERD-ERS/
        └── TASK.md                           report to be added
```

Each `report/latex/` folder is self-contained (`main.tex`, `sections/`, `figures/`) and can be uploaded to Overleaf as it is.

---

## 1. Biosignal time-frequency analysis

**Directory:** [`Biosignal-Time-Frequency-Analysis`](<Medical Signal and Image Processing Lab/Biosignal-Time-Frequency-Analysis>) · **Task:** [`TASK.md`](<Medical Signal and Image Processing Lab/Biosignal-Time-Frequency-Analysis/TASK.md>) · **Report:** [`Report_EN.pdf`](<Medical Signal and Image Processing Lab/Biosignal-Time-Frequency-Analysis/report/Report_EN.pdf>)

**Objective:** examine how the time-domain waveform, the one-sided DFT amplitude spectrum, the Welch power spectral density (PSD) and the short-time Fourier transform (STFT) spectrogram relate to each other across four electrophysiological modalities.

### Datasets

| Modality | File | Channels | Sampling rate | Duration (samples) | Context |
|----------|------|----------|---------------|--------------------|---------|
| EEG | `EEG_sig.mat` | 32 | 256 Hz | 64.0 s (16,384) | Epileptic seizure recording; focus on channel 5 (`C3`) |
| ECG | `ECG_sig.mat` | 2 (`MLII`, `V1`) | 360 Hz | 1805.556 s (650,000) | MIT-BIH record 201 (200 ADU/mV, baseline 1024) |
| EOG | `EOG_sig.mat` | 2 (left, right) | 128 Hz | 60.0 s (7,680) | Bilateral eye-movement recording |
| EMG | `EMG_sig.mat` | 3 subjects | 4000 Hz (from 50 kHz) | 12.715 / 27.584 / 36.965 s | Needle EMG of the *tibialis anterior*: healthy, myopathy, neuropathy |

### Key findings

**Seizure EEG (`C3`) and anti-aliased downsampling**

Four 5-s segments (1280 samples, Δf = 0.2 Hz) were analyzed:

| Segment (s) | Pattern in the manual's three-phase figure | AC RMS (a.u.) | Welch peak | Beta share (13-30 Hz) |
|-------------|---------------------------------------------|---------------|------------|-----------------------|
| 2-7 | Background | 4.58 | 5.625 Hz | 21.97 % |
| 30-35 | Rhythmic spikes ("before seizure onset") | 9.61 | 2.875 Hz | 52.92 % |
| 42-47 | Fast activity | 12.14 | 16.375 Hz | 64.82 % |
| 50-55 | Slower, irregular activity | 10.81 | 2.375 Hz | 47.32 % |

- Spectrograms used a 128-sample Hamming window (0.5 s), 64-sample overlap (0.25 s hop) and NFFT = 128 (Δf = 2 Hz).
- Downsampling segment 30-35 s from 256 to 128 Hz after a zero-phase (`filtfilt`) low-pass filter (pass 48 Hz, stop 64 Hz, 0.1 dB ripple, 60 dB single-pass / 120 dB effective attenuation) kept 96.56 % of the raw power below 48 Hz, with sample correlation r = 0.9862, RMSE = 1.609 a.u. and a 0.327 % relative Welch error over 0.5-40 Hz.

**Arrhythmia ECG (MIT-BIH record 201)**

- The mismatched 720,000-element `TIME` vector was discarded and the time axis rebuilt as t = n / 360 s for the 650,000 samples.
- **1,963 ventricular complexes** were separated from **76 non-beat annotations**:
  - beats: 1625 `N`, 198 `V`, 97 `a`, 30 `A`, 10 `j`, 2 `F`, 1 `J` (17.22 % non-`N`);
  - non-beat events: 37 `x` (non-conducted atrial P waves), 35 `+` (rhythm changes), 4 `~` (signal quality changes).
- Mean RR interval 0.920 s (median 0.863 s, range 0.336-3.772 s), giving 65.23 bpm over the whole recording.
- Consecutive normal (`N-N-N`, 3.542 s) and abnormal (`V-a-a`, 1.825 s) three-beat segments have 95 % Welch-power bandwidths of 19.86 Hz vs. 10.46 Hz in `MLII` (19.60 Hz vs. 17.40 Hz in `V1`): beat rate is not the same as QRS frequency content.

**Eye movements (EOG)**

- The two channels are negatively correlated (r = −0.409 raw, −0.424 after a 5 Hz low-pass), consistent with a lateral-electrode arrangement with a shared reference; the signals alone cannot confirm the exact electrode positions.
- 98.08 % (left) and 98.00 % (right) of the Welch power lies below 5 Hz.

**Needle EMG (first 10 s, 40,000 samples per subject)**

| Condition | AC RMS (a.u.) | Mean frequency | Median frequency | Welch peak |
|-----------|---------------|----------------|------------------|------------|
| Healthy | 820.08 | 166.49 Hz | 63.31 Hz | 6.84 Hz |
| Myopathy | 962.72 | 429.21 Hz | 238.52 Hz | 28.32 Hz |
| Neuropathy | 2325.40 | 476.86 Hz | 346.40 Hz | 172.85 Hz |

These indices describe the three recordings only; they are not general diagnostic criteria.

<p align="center">
<img src="Medical%20Signal%20and%20Image%20Processing%20Lab/Biosignal-Time-Frequency-Analysis/report/latex/figures/fig08_welch_psd_segments.png" width="48%" alt="Welch PSD of four C3 segments">
<img src="Medical%20Signal%20and%20Image%20Processing%20Lab/Biosignal-Time-Frequency-Analysis/report/latex/figures/fig28_emg_spectra.png" width="48%" alt="Spectra of the three EMG recordings">
</p>

---

## 2. EEG denoising with independent component analysis (ICA)

**Directory:** [`EEG-Denoising-with-ICA`](<Medical Signal and Image Processing Lab/EEG-Denoising-with-ICA>) · **Task:** [`TASK.md`](<Medical Signal and Image Processing Lab/EEG-Denoising-with-ICA/TASK.md>) · **Report:** [`Report_EN.pdf`](<Medical Signal and Image Processing Lab/EEG-Denoising-with-ICA/report/Report_EN.pdf>)

**Objective:** apply fourth-order cumulant ICA (`CoM2` through `COM2R.m`) to recover spiky neural sources from simulated noisy mixtures, and to remove physiological and instrumental artifacts from real 21-channel EEG while conservatively keeping ambiguous neural components.

### Method

- **Decomposition:** the mean-centered observations X<sub>c</sub> = X − μ**1**<sup>T</sup> (f<sub>s</sub> = 250 Hz) are decomposed by `COM2R.m` (SVD whitening followed by pairwise Jacobi rotations; 7 sweeps for 32 channels, 6 sweeps for 21 channels) into X<sub>c</sub> = A S. The columns of A are the scalp projections shown with `plottopomap.m` and `Electrodes.mat`.
- **Component features:** Welch PSD (2-s Hamming window, 50 % overlap, NFFT = 1024), excess kurtosis κ = E[z⁴] − 3, high-frequency ratio HF = P(30-100 Hz) / P(0.5-100 Hz), low-frequency ratio LF = P(0.5-4 Hz) / P(0.5-100 Hz), and the 60 Hz line-power ratio (59-61 Hz).

### Part 1: simulated 32-channel interictal spike denoising

`X_org.mat` plus σ·`X_noise.mat`, 40.96 s. Spiky sources were selected blindly, without tuning on `X_org`, with the joint criterion κ ≥ 6, max |z(t)| ≥ 6 and HF ≤ 0.20:

| Input SNR | σ | Retained ICs | Input RRMSE | Output RRMSE | Output SNR | SNR gain |
|-----------|---|--------------|-------------|--------------|------------|----------|
| −5 dB | 3.5822 | 8 / 32 (2, 5, 9, 10, 12, 21, 23, 32) | 1.7783 | 0.7147 | +2.92 dB | +7.92 dB |
| −15 dB | 11.3280 | 6 / 32 (7, 8, 18, 20, 30, 32) | 5.6234 | 2.0843 | −6.38 dB | +8.62 dB |

> At −15 dB the absolute error stays large (RRMSE 2.0843) and some `T3` spikes are only partly recovered, even with an 8.62 dB gain. The result illustrates the limits of blind separation under strong non-stationary muscle noise.

### Part 2: real 21-channel EEG artifact removal

`NewData1.mat` and `NewData3.mat` (5.0 s each):

| Dataset | Rejected ICs | Evidence and conservative retention | Removed energy ratio | 30-100 Hz attenuation | 0.5-4 Hz change | Median channel correlation |
|---------|--------------|--------------------------------------|----------------------|-----------------------|-----------------|----------------------------|
| `NewData1` | IC4 | Narrow-band 60.059 Hz line interference (75.1 % of its power in 59-61 Hz, HF = 0.768); the ECG-like IC7 is kept | 0.0809 | 5.80 dB | −0.02 dB | 0.9640 |
| `NewData3` | IC1, IC8 | IC1: bilateral frontal blink-like transients (LF = 0.816, frontal share 0.640); IC8: focal high-frequency noise on `T4` (HF = 0.805, weight concentration 0.971) | 0.3353 | 2.27 dB | 3.81 dB | 0.9098 |

There is no clean reference for the real data, so these indices only describe the change; they do not measure reconstruction accuracy.

<p align="center">
<img src="Medical%20Signal%20and%20Image%20Processing%20Lab/EEG-Denoising-with-ICA/report/latex/figures/fig04_channels_t3_fc5_m5db.png" width="48%" alt="Channels T3 and FC5 at -5 dB">
<img src="Medical%20Signal%20and%20Image%20Processing%20Lab/EEG-Denoising-with-ICA/report/latex/figures/fig08_rrmse_and_snr.png" width="48%" alt="RRMSE and SNR at both noise levels">
</p>

---

## 3. Maternal-fetal ECG separation (SVD vs. ICA)

**Directory:** [`Maternal-Fetal-ECG-Separation-SVD-vs-ICA`](<Medical Signal and Image Processing Lab/Maternal-Fetal-ECG-Separation-SVD-vs-ICA>) · **Task:** [`TASK.md`](<Medical Signal and Image Processing Lab/Maternal-Fetal-ECG-Separation-SVD-vs-ICA/TASK.md>) · **Report:** [`Report_EN.pdf`](<Medical Signal and Image Processing Lab/Maternal-Fetal-ECG-Separation-SVD-vs-ICA/report/Report_EN.pdf>)

**Objective:** contrast second-order orthogonal decomposition (SVD/PCA) with fourth-order statistical independence (JADE ICA) when extracting a weak fetal ECG from a 3-channel abdominal mixture (M = 2560 samples, N = 3 channels, f<sub>s</sub> = 256 Hz, 10 s).

### Source statistics and decompositions

- **Part 1, sources (`mecg1.dat`, `fecg1.dat`, `noise1.dat`):** all three are zero-mean with unit sample variance (about 1.000 mV²), but differ in higher-order statistics and spectrum: maternal ECG (β₂ = 14.042, sharp roll-off above 20 Hz), fetal ECG (β₂ = 8.990, energy reaching higher frequencies) and near-Gaussian noise (β₂ = 2.766).
- **Part 2, SVD (`X.dat`):** the economy SVD X<sub>c</sub> = UΣV<sup>T</sup> gives singular values [182.9290, 75.2962, 55.4650], covariance eigenvalues [13.0766, 2.2155, 1.2022] mV² and variance shares 79.28 %, 13.43 % and 7.29 %. Component 3 (σ₃u₃v₃<sup>T</sup>, only 7.29 % of the variance) isolates the fast fetal QRS complexes best, so ranking by variance does not identify the target source.
- **Part 3, ICA with JADE (`X.dat`):** joint diagonalization of fourth-order cumulant matrices separates the maternal component (IC1: 62.69 bpm, β₂ = 14.56, CV<sub>RR</sub> = 0.0757), the noise (IC2: β₂ = 2.85, CV<sub>RR</sub> = 0.7017) and the fetal component (IC3: 155.15 bpm, β₂ = 8.93, CV<sub>RR</sub> = 0.0728).
- **Part 4, geometry:** the PCA axes are orthogonal (90°), whereas the estimated ICA mixing columns are not (θ₁₂ = 62.03°, θ₁₃ = 71.34°, θ₂₃ = 37.61°). The third PCA direction v₃ and the ICA fetal mixing vector â₃ differ by 70.86°.

### Reconstruction against the ground-truth fetal reference

Channel 1 versus `fecg2.dat`, using Pearson correlation |ρ| and an explicit post-hoc least-squares scale alignment α\* = yᵀr / yᵀy (α\*<sub>SVD</sub> = −1.2686, α\*<sub>ICA</sub> = 0.6163). The fit plays no role in training the separators.

| Method | Selected component | \|ρ\| | Scale-aligned NRMSE | Scale-aligned MSE | Aligned SNR |
|--------|--------------------|-------|---------------------|-------------------|-------------|
| SVD / PCA | Component 3 (7.29 % of the variance) | 0.6749 | 0.7379 | 0.34831 | 2.64 dB |
| ICA (JADE) | Component 3 (155.15 bpm) | 0.9978 | 0.0657 | 0.00276 (126× lower) | 23.65 dB |

The evaluation uses one simulated 10-s record, so it shows what each method does on this data rather than how it generalizes.

<p align="center">
<img src="Medical%20Signal%20and%20Image%20Processing%20Lab/Maternal-Fetal-ECG-Separation-SVD-vs-ICA/report/latex/figures/fig08_svd_fetal_estimate.png" width="48%" alt="Fetal estimate with SVD">
<img src="Medical%20Signal%20and%20Image%20Processing%20Lab/Maternal-Fetal-ECG-Separation-SVD-vs-ICA/report/latex/figures/fig11_ica_fetal_estimate.png" width="48%" alt="Fetal estimate with ICA/JADE">
</p>

---

## Reports

Each experiment has an English report (`report/Report_EN.pdf`) with every figure, table and equation of the original Persian report, plus the translated task statement above each answer. The LaTeX source is in `report/latex/`:

```bash
cd "Medical Signal and Image Processing Lab/<experiment>/report/latex"
pdflatex main.tex && pdflatex main.tex
```

`pdflatex` with a standard TeX Live installation is sufficient, and the folder also works on Overleaf (set `main.tex` as the main document). The author, instructor and semester shown on the title pages are set in `authors.tex`.

## Prerequisites and usage

### MATLAB (Experiments 1 and 2, helper utilities in Experiment 3)

- MATLAB R2018b or newer (the reports were produced with R2025b) and the **Signal Processing Toolbox**.
- Add the experiment's `data/` directory to the path with `addpath('...')`, load the `.mat` or `.dat` files, and call the supplied functions (`disp_eeg.m`, `COM2R.m`, `plottopomap.m`, `plotEEG.m`, `jade.m`, `ica.m`, `plot3ch.m`, `plot3dv.m`) with the windowing and decomposition parameters documented in each report.

### Python (Experiment 3 analysis pipeline)

- Python 3.10+ (tested on 3.14.0) with `numpy` (>= 2.0), `scipy` (>= 1.14) and `matplotlib` (>= 3.9).
- Load `X.dat` and `fecg2.dat` with `numpy.loadtxt`, subtract the channel means, run the economy SVD (`numpy.linalg.svd(..., full_matrices=False)`) and JADE ICA, and compute the channel-wise correlation and the scale-aligned NRMSE/SNR against `fecg2.dat`.

## Data and course materials

The datasets and helper functions in each `data/` folder were supplied by the course staff of Sharif University of Technology and remain the property of their original authors. Two of the datasets come from public PhysioNet databases:

- MIT-BIH Arrhythmia Database (record 201): <https://physionet.org/content/mitdb/>
- PhysioNet EMG Examples Database: <https://archive.physionet.org/physiobank/database/emgdb>

The `TASK.md` files are English translations of the lab manuals, included to document each experiment.

## License

The original code, reports and documentation in this repository are released under the [MIT License](LICENSE). Course-provided datasets, helper functions and manuals are excluded and keep their original terms.

This repository documents coursework and is shared for reference and learning. If you are taking the course, use it to understand the methods, not as a substitute for your own work.
