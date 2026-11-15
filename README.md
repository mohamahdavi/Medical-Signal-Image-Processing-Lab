# Medical Signal and Image Processing Lab

This repository contains MATLAB implementations, datasets, helper utilities, and technical reports for core biomedical signal processing experiments. The projects focus on time-frequency analysis of physiological signals and Blind Source Separation (BSS) techniques—specifically Singular Value Decomposition (SVD) and Independent Component Analysis (ICA)—for artifact removal and fetal ECG extraction.

---

## Repository Structure

```text
Medical-Signal-Image-Processing-Lab/
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
    │   │   ├── Electrodes.mat
    │   │   ├── NewData1.mat - NewData4.mat
    │   │   ├── X_noise.mat
    │   │   ├── X_org.mat
    │   │   ├── disp_eeg.m
    │   │   ├── plotEEG.m
    │   │   ├── plottopomap.m
    │   │   └── tfuni4.m
    │   └── report/
    │       └── Report_EN.pdf
    │
    └── Maternal-Fetal-ECG-Separation-SVD-vs-ICA/
        ├── data/
        │   ├── A.dat, X.dat, Z.dat
        │   ├── fecg1.dat, fecg2.dat
        │   ├── mecg1.dat, mecg2.dat
        │   ├── noise1.dat, noise2.dat
        │   ├── observation.dat
        │   ├── ica.m
        │   ├── jade.m
        │   ├── plot3ch.m
        │   └── plot3dv.m
        └── report/
            └── Report_EN.pdf
```

---

## Laboratory Experiments

### 1. Biosignal Time-Frequency Analysis
* **Directory:** `Medical Signal and Image Processing Lab/Biosignal-Time-Frequency-Analysis`
* **Objective:** Analyze the temporal and spectral characteristics of major electrophysiological signals, including Electrocardiogram (ECG), Electroencephalogram (EEG), Electromyogram (EMG), and Electrooculogram (EOG).
* **Key Components:**
  * **Datasets:** Real-world recordings (`ECG_sig.mat`, `EEG_sig.mat`, `EMG_sig.mat`, `EOG_sig.mat`) accompanied by MIT-BIH Arrhythmia documentation and signal annotations.
  * **Utilities:** `disp_eeg.m` for multi-channel EEG visualization.
  * **Report:** Full methodology, spectral plots, and time-frequency findings in `report/Report_EN.pdf`.

### 2. EEG Denoising with Independent Component Analysis (ICA)
* **Directory:** `Medical Signal and Image Processing Lab/EEG-Denoising-with-ICA`
* **Objective:** Identify, isolate, and remove physiological artifacts (such as ocular and muscular noise) from multi-channel EEG recordings using Blind Source Separation.
* **Key Components:**
  * **Algorithms & Utilities:**
    * `COM2R.m` & `EMGsorsep.m`: ICA-based source separation routines.
    * `plottopomap.m`: Spatial topographic scalp mapping of independent components using electrode coordinates (`Electrodes.mat`).
    * `plotEEG.m` & `disp_eeg.m`: Time-domain multi-channel EEG plotting tools.
    * `tfuni4.m`: Time-frequency representation utilities for component inspection.
  * **Datasets:** Clean reference signals (`X_org.mat`), noisy mixtures (`X_noise.mat`), and experimental multi-channel EEG recordings (`NewData1.mat` to `NewData4.mat`).
  * **Report:** Component selection criteria and denoising evaluation in `report/Report_EN.pdf`.

### 3. Maternal-Fetal ECG Separation (SVD vs. ICA)
* **Directory:** `Medical Signal and Image Processing Lab/Maternal-Fetal-ECG-Separation-SVD-vs-ICA`
* **Objective:** Compare second-order statistical methods (Singular Value Decomposition) against higher-order statistical methods (Independent Component Analysis) in extracting weak fetal ECG signals from non-invasive abdominal recordings dominated by maternal ECG and background noise.
* **Key Components:**
  * **Algorithms & Utilities:**
    * `ica.m` & `jade.m`: Standard ICA and Joint Approximate Diagonalization of Eigen-matrices (JADE) implementations.
    * `plot3ch.m` & `plot3dv.m`: 3-channel time-series and 3D vector-space visualization of mixed and separated sources.
  * **Datasets:** Ground-truth maternal signals (`mecg1.dat`, `mecg2.dat`), fetal signals (`fecg1.dat`, `fecg2.dat`), noise components (`noise1.dat`, `noise2.dat`), mixing matrix (`A.dat`), and composite observations (`X.dat`, `Z.dat`, `observation.dat`).
  * **Report:** Quantitative and visual comparison between SVD and ICA separation performance in `report/Report_EN.pdf`.

---

## Prerequisites & Usage

1. **Software Requirements:**
   * MATLAB (R2018b or newer recommended)
   * Signal Processing Toolbox
2. **Running the Experiments:**
   * Navigate to the desired experiment's `data/` folder in MATLAB so that the helper `.m` functions and `.mat`/`.dat` datasets are on the active path.
   * Load the corresponding `.mat` or `.dat` files into the workspace and run the visualization/separation scripts as detailed in each experiment's `report/Report_EN.pdf`.
