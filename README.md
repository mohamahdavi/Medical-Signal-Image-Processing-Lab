# Medical Signal and Image Processing Lab

This repository contains implementations, biomedical datasets, helper utilities, and technical reports for the **Medical Signal and Image Processing Laboratory** at **Sharif University of Technology** (Fall 2026).

* **Instructor:** Marzieh Barakat
* **Authors:** Mohammadreza Mahdavi, Reza Kazemini Moghaddam

The laboratory covers time-frequency analysis of electrophysiological signals and Blind Source Separation (BSS) techniques—contrasting second-order Singular Value Decomposition (SVD/PCA) against fourth-order Independent Component Analysis (`CoM2` and `JADE`)—for artifact removal and non-invasive fetal ECG extraction.

---

## Repository Structure

```text
Medical-Signal-Image-Processing-Lab/
├── README.md
└── Medical Signal and Image Processing Lab/
    ├── Biosignal-Time-Frequency-Analysis/
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
    │       └── Report_EN.pdf
    │
    ├── EEG-Denoising-with-ICA/
    │   ├── data/
    │   │   ├── COM2R.m
    │   │   ├── EMGsorsep.m
    │   │   ├── Electrodes (2).mat
    │   │   ├── Electrodes.mat
    │   │   ├── NewData1.mat
    │   │   ├── NewData2.mat
    │   │   ├── NewData3.mat
    │   │   ├── NewData4.mat
    │   │   ├── X_noise.mat
    │   │   ├── X_org.mat
    │   │   ├── disp_eeg (2).m
    │   │   ├── disp_eeg.m
    │   │   ├── plotEEG (2).m
    │   │   ├── plotEEG.m
    │   │   ├── plottopomap.m
    │   │   └── tfuni4.m
    │   └── report/
    │       └── Report_EN.pdf
    │
    └── Maternal-Fetal-ECG-Separation-SVD-vs-ICA/
        ├── data/
        │   ├── A.dat
        │   ├── X.dat
        │   ├── Z.dat
        │   ├── fecg1.dat
        │   ├── fecg2.dat
        │   ├── mecg1.dat
        │   ├── mecg2.dat
        │   ├── noise1.dat
        │   ├── noise2.dat
        │   ├── observation.dat
        │   ├── ica.m
        │   ├── jade.m
        │   ├── plot3ch.m
        │   └── plot3dv.m
        └── report/
            └── Report_EN.pdf
```

---

## Laboratory Experiments & Quantitative Summary

### 1. Biosignal Time-Frequency Analysis

* **Directory:** `Medical Signal and Image Processing Lab/Biosignal-Time-Frequency-Analysis`
* **Objective:** Examine the relationship between time-domain waveform morphology, one-sided DFT amplitude spectra, Welch Power Spectral Density (PSD), and Short-Time Fourier Transform (STFT) spectrograms across four electrophysiological modalities.

#### Datasets & Physical Calibration

| Modality | File | Channels | Sampling Rate ($f_s$) | Duration / Samples | Calibration & Clinical Context |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **EEG** | `EEG_sig.mat` | $32$ | $256\text{ Hz}$ | $64.0\text{ s}$ ($16,384$) | Pre-ictal and ictal recording; primary focus on channel 5 (`C3`) |
| **ECG** | `ECG_sig.mat` | $2$ (`MLII`, `V1`) | $360\text{ Hz}$ | $1805.556\text{ s}$ ($650,000$) | MIT-BIH Record 201 ($200\text{ ADU/mV}$, baseline $1024$) |
| **EOG** | `EOG_sig.mat` | $2$ (Left, Right) | $128\text{ Hz}$ | $60.0\text{ s}$ ($7,680$) | Bilateral ocular potential recordings |
| **EMG** | `EMG_sig.mat` | $1 \times 3$ subjects | $4000\text{ Hz}$ | $12.715\text{ s}$ / $27.584\text{ s}$ / $36.965\text{ s}$ | Needle EMG (*tibialis anterior*): Healthy, Myopathy, Neuropathy |

#### Key Findings & Numerical Results

* **Seizure EEG (`C3`) & Anti-Aliased Downsampling:**
  * Analyzed four $5\text{-s}$ windows ($N = 1280$, $\Delta f = 0.2\text{ Hz}$):
    * **Baseline `[2, 7) s`:** $\text{AC RMS} = 4.58\text{ a.u.}$, Welch peak at $5.625\text{ Hz}$, beta-band ($13\text{-}30\text{ Hz}$) power share $= 21.97\%$.
    * **Rhythmic Pre-Ictal Spikes `[30, 35) s`:** $\text{AC RMS} = 9.61\text{ a.u.}$, Welch peak at $2.875\text{ Hz}$, beta-band power share $= 52.92\%$.
    * **Fast Ictal Activity `[42, 47) s`:** $\text{AC RMS} = 12.14\text{ a.u.}$, Welch peak at $16.375\text{ Hz}$, beta-band power share $= 64.82\%$.
    * **Slower Irregular Activity `[50, 55) s`:** $\text{AC RMS} = 10.81\text{ a.u.}$, Welch peak at $2.375\text{ Hz}$, beta-band power share $= 47.32\%$.
  * STFT spectrograms were computed using a $128$-sample Hamming window ($0.5\text{ s}$), $64$-sample overlap ($0.25\text{ s}$ hop), and $\text{NFFT} = 128$ ($\Delta f = 2\text{ Hz}$).
  * Downsampling from $256\text{ Hz}$ to $128\text{ Hz}$ on `[30, 35) s` after zero-phase (`filtfilt`) low-pass filtering ($f_{\text{pass}} = 48\text{ Hz}$, $f_{\text{stop}} = 64\text{ Hz}$, $0.1\text{ dB}$ passband ripple, $60\text{ dB}$ single-pass / $120\text{ dB}$ effective stopband attenuation) preserved $96.56\%$ of raw signal power below $48\text{ Hz}$, achieving $r = 0.9862$ sample correlation, $\text{RMSE} = 1.609\text{ a.u.}$, and $0.327\%$ relative Welch PSD error across $0.5\text{-}40\text{ Hz}$.

* **Arrhythmia ECG (MIT-BIH Record 201):**
  * Discarded the mismatched $720,000$-element `TIME` vector and reconstructed $t_n = n / 360\text{ s}$ for the $650,000$-sample recording.
  * Separated **1,963 true ventricular beats** (`1625 N`, `198 V`, `97 a`, `30 A`, `10 j`, `2 F`, `1 J`; $17.22\%$ non-`N`) from **76 non-beat annotations** (`37 X` blocked atrial P-waves, `35 +` rhythm changes, `4 ~` signal quality changes). Mean $\text{RR}$ interval was $0.920\text{ s}$ (median $0.863\text{ s}$, range $0.336\text{-}3.772\text{ s}$), yielding an overall heart rate of $65.23\text{ bpm}$.
  * Comparing consecutive normal (`N-N-N`, $3.542\text{ s}$, $\Delta f = 0.2824\text{ Hz}$) vs. abnormal (`V-a-a`, $1.825\text{ s}$, $\Delta f = 0.5479\text{ Hz}$) segments showed $95\%$ Welch power bandwidths in `MLII` of $19.86\text{ Hz}$ vs. $10.46\text{ Hz}$ ($19.60\text{ Hz}$ vs. $17.40\text{ Hz}$ in `V1`), confirming that faster ventricular rate does not imply higher QRS frequency content.

* **Ocular EOG:**
  * Exhibited negative inter-channel correlation ($r = -0.409$ raw; $r = -0.424$ low-pass filtered below $5\text{ Hz}$), consistent with bilateral horizontal electrodes around a shared reference. Over $98\%$ of Welch PSD power resided below $5\text{ Hz}$ ($98.08\%$ Left, $98.00\%$ Right).

* **Needle EMG (First $10\text{ s}$, $40,000$ Samples per Subject):**

| Condition | AC RMS ($\text{a.u.}$) | Mean Frequency ($\text{MNF}$) | Median Frequency ($\text{MDF}$) | Welch Peak |
| :--- | :--- | :--- | :--- | :--- |
| **Healthy** | $820.08$ | $166.49\text{ Hz}$ | $63.31\text{ Hz}$ | $6.84\text{ Hz}$ |
| **Myopathy** | $962.72$ | $429.21\text{ Hz}$ | $238.52\text{ Hz}$ | $28.32\text{ Hz}$ |
| **Neuropathy** | $2325.40$ | $476.86\text{ Hz}$ | $346.40\text{ Hz}$ | $172.85\text{ Hz}$ |

---

### 2. EEG Denoising with Independent Component Analysis (ICA)

* **Directory:** `Medical Signal and Image Processing Lab/EEG-Denoising-with-ICA`
* **Objective:** Apply fourth-order cumulant ICA (`CoM2` via `COM2R.m`) to recover spiky neural sources in simulated noisy mixtures and remove physiological/instrumental artifacts from real $21$-channel EEG recordings while conservatively retaining ambiguous neural components.

#### Methodology & Helper Scripts

* **Decomposition:** Mean-centered observations $X_c = X - \mu \mathbf{1}^T$ ($f_s = 250\text{ Hz}$) are decomposed via `COM2R.m` (SVD whitening followed by pairwise Jacobi rotations; $7$ sweeps for $32$ channels, $6$ sweeps for $21$ channels) into $X_c = A S$, where columns of $A$ define sensor-space scalp projections (`plottopomap.m` with `Electrodes.mat`).
* **Spectral & Statistical Features:** Welch PSD ($2\text{-s}$ Hamming window, $50\%$ overlap, $\text{NFFT} = 1024$), excess kurtosis $\kappa = E[z^4] - 3$, high-frequency power ratio $HF = P_{30\text{-}100\text{ Hz}} / P_{0.5\text{-}100\text{ Hz}}$, low-frequency power ratio $LF = P_{0.5\text{-}4\text{ Hz}} / P_{0.5\text{-}100\text{ Hz}}$, and $60\text{ Hz}$ line-power ratio ($59\text{-}61\text{ Hz}$).

#### Part 1: Simulated 32-Channel Interictal Spike Denoising (`X_org.mat` + $\sigma$ `X_noise.mat`, $40.96\text{ s}$)

Spiky sources were selected blindly (without using `X_org` to tune thresholds) via the joint criterion $\kappa \ge 6 \;\land\; \max_t |z(t)| \ge 6 \;\land\; HF \le 0.20$:

| Input SNR | Noise Scale ($\sigma$) | Retained ICs | Input RRMSE | Output RRMSE | Output SNR | SNR Gain |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| $-5\text{ dB}$ | $3.5822$ | `8 / 32` (`2, 5, 9, 10, 12, 21, 23, 32`) | $1.7783$ | $0.7147$ | $+2.92\text{ dB}$ | $+7.92\text{ dB}$ |
| $-15\text{ dB}$ | $11.3280$ | `6 / 32` (`7, 8, 18, 20, 30, 32`) | $5.6234$ | $2.0843$ | $-6.38\text{ dB}$ | $+8.62\text{ dB}$ |

*(Note: At $-15\text{ dB}$, despite an $8.62\text{ dB}$ relative SNR gain, absolute RRMSE remains $2.0843$ with partial attenuation of temporal spikes in `T3`, illustrating BSS limitations under severe non-stationary muscular noise.)*

#### Part 2: Real 21-Channel EEG Artifact Removal (`NewData1.mat` & `NewData3.mat`, $5.0\text{ s}$)

| Dataset | Rejected ICs | Artifact Evidence & Conservative Retention | Removed Energy Ratio | High-Freq Attenuation ($30\text{-}100\text{ Hz}$) | Low-Freq Attenuation ($0.5\text{-}4\text{ Hz}$) | Median Channel Correlation |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`NewData1`** | `IC4` | Narrowband $60.059\text{ Hz}$ line interference ($75.1\%$ power in $59\text{-}61\text{ Hz}$, $HF = 0.768$); ambiguous ECG-like `IC7` kept | $0.0809$ | $5.80\text{ dB}$ | $-0.02\text{ dB}$ | $0.9640$ |
| **`NewData3`** | `IC1`, `IC8` | `IC1`: bilateral frontal blinks ($LF = 0.816$, frontal share $0.640$); `IC8`: focal high-freq noise on **`T4`** ($HF = 0.805$, weight concentration $0.971$) | $0.3353$ | $2.27\text{ dB}$ | $3.81\text{ dB}$ | $0.9098$ |

---

### 3. Maternal-Fetal ECG Separation (SVD vs. ICA)

* **Directory:** `Medical Signal and Image Processing Lab/Maternal-Fetal-ECG-Separation-SVD-vs-ICA`
* **Objective:** Contrast second-order orthogonal decomposition (SVD/PCA) with fourth-order statistical independence (`JADE` ICA) in extracting weak fetal ECG signals from $3$-channel abdominal mixtures ($M = 2560$ samples, $N = 3$ channels, $f_s = 256\text{ Hz}$, $10.0\text{ s}$).

#### Source Statistics & Decomposition Analysis

* **Part 1 Source Characteristics (`mecg1.dat`, `fecg1.dat`, `noise1.dat`):** All three sources are zero-mean with unit sample variance ($s^2 \approx 1.000\text{ mV}^2$) but differ in higher-order statistics and spectral roll-off: maternal ECG ($\beta_2 = 14.042$, sharp attenuation above $20\text{ Hz}$), fetal ECG ($\beta_2 = 8.990$, spectral energy extending to higher frequencies), and near-Gaussian noise ($\beta_2 = 2.766$).
* **Part 2 SVD (`X.dat`):** Economy SVD ($X_c = U \Sigma V^T$) yields singular values $\sigma = [182.9290, 75.2962, 55.4650]$, covariance eigenvalues $\lambda = [13.0766, 2.2155, 1.2022]\text{ mV}^2$, and variance shares of $79.28\%$, $13.43\%$, and $7.29\%$. Component 3 ($\sigma_3 u_3 v_3^T$, only $7.29\%$ variance) best isolates the faster fetal QRS complexes, demonstrating that variance ranking does not identify the target physiological source.
* **Part 3 ICA via JADE (`X.dat`):** Joint diagonalization of fourth-order cumulant matrices separates the mixture into Maternal (`IC1`: $\text{HR} = 62.69\text{ bpm}$, $\beta_2 = 14.56$, $\text{CV}_{\text{RR}} = 0.0757$), Noise (`IC2`: $\beta_2 = 2.85$, $\text{CV}_{\text{RR}} = 0.7017$), and Fetal (`IC3`: $\text{HR} = 155.15\text{ bpm}$, $\beta_2 = 8.93$, $\text{CV}_{\text{RR}} = 0.0728$).
* **Part 4 Geometric Comparison:** Unlike orthogonal PCA axes ($\theta = 90^\circ$), estimated ICA mixing columns are non-orthogonal ($\theta_{1,2} = 62.03^\circ$, $\theta_{1,3} = 71.34^\circ$, $\theta_{2,3} = 37.61^\circ$), with a $70.86^\circ$ angular divergence between PCA's third direction ($v_3$) and ICA's fetal mixing vector ($\hat{a}_3$).

#### Reconstruction Performance vs. Ground-Truth Fetal Reference (`fecg2.dat`, Channel 1)

Evaluated using Pearson correlation $|\rho|$ and explicit post-hoc least-squares scale alignment ($\alpha^* = y^T r / y^T y$, where $\alpha^*_{\text{SVD}} = -1.2686$ and $\alpha^*_{\text{ICA}} = 0.6163$):

| Method | Selected Component | Abs. Correlation ($|\rho|$) | Scale-Aligned NRMSE | Scale-Aligned MSE | Aligned SNR |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **SVD / PCA** | Component 3 ($7.29\%$ var.) | $0.6749$ | $0.7379$ | $0.34831$ | $2.64\text{ dB}$ |
| **ICA (JADE)** | Component 3 ($\text{HR} = 155.15\text{ bpm}$) | $0.9978$ | $0.0657$ | $0.00276$ ($126\times$ lower) | $23.65\text{ dB}$ |

---

## Prerequisites & Usage

### MATLAB Environment (Labs 1, 2 & Reference Utilities in Lab 3)

* **Requirements:** MATLAB (R2018b or newer; tested on R2025) with the **Signal Processing Toolbox**.
* **Execution:**
  1. Add the target experiment's `data/` directory to your MATLAB path using `addpath('...')`.
  2. Load the `.mat` or `.dat` datasets and invoke the provided visualization and separation functions (`disp_eeg.m`, `COM2R.m`, `plottopomap.m`, `plotEEG.m`, `jade.m`, `ica.m`, `plot3ch.m`, `plot3dv.m`) using the exact windowing and decomposition parameters documented in each experiment's `report/Report_EN.pdf`.

### Python Environment (Lab 3 Analysis Pipeline)

* **Requirements:** Python 3.10+ (tested on Python 3.14.0) with `numpy` ($\ge 2.0$), `scipy` ($\ge 1.14$), and `matplotlib` ($\ge 3.9$).
* **Execution:** Load `X.dat` and `fecg2.dat` via `numpy.loadtxt`, subtract channel means, perform economy SVD (`numpy.linalg.svd(..., full_matrices=False)`) alongside `JADE` ICA, and compute channel-wise correlation and least-squares scale-aligned NRMSE/SNR against `fecg2.dat`.
