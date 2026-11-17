# EEG Noise and Artifact Removal with ICA

**Medical Signal and Image Processing Lab, Experiment 2 (English translation of the lab manual)**

**Original title:** Noise and artifacts of the EEG signal and methods for removing them.

---

## Part 1: Denoising of Simulated Non-Seizure Epileptic Signals

In this part, we use the ICA method to study noise removal on a simulated EEG signal (the data of this part are in the folder `Lab2_1`). Since we have the noise-free data (the desired signal) in the simulated setting, we can evaluate and compare different algorithms with quantitative metrics. For this purpose we use a 32-channel interictal (non-seizure) epileptic EEG signal. This realistic simulated data is the noise-free data (the desired signal), stored in the matrix `X_org`. A real signal containing muscle noise and background EEG is also provided (`X_noise`). We want to add the noise to the desired signal at different SNRs and then reconstruct the desired signal using blind source separation algorithms.

1. Plot the noise-free signal `X_org`. Label the channels and the time. You can use the files `plotEEG.m` and `eeg_disp.m`. For a better display, you can tune the variable `offset` (the spacing between the signals shown for two channels).
2. Plot the noise `X_noise`. Label the channels and the time.
3. Add the noise to the original signal at different SNRs (−5 dB and −15 dB) and build the observation signal. Plot the noisy signal for SNR = −15 dB and compare it with the noise-free signal.

   **Notes on SNR:**

   $$SNR_{dB} = 10\log_{10}(SNR) = 10\log_{10}\left(\frac{P_{signal}}{P_{noise}}\right)$$

   - SNR of a multichannel signal, with $\mathbf{x}[m] = \mathbf{s}[m] + \mathbf{N}[m]$:

     $$SNR_{dB} = 10\log_{10}\left(\frac{\sum_{n=1}^{N}\sum_{m=1}^{M} s_n[m]^2}{\sum_{n=1}^{N}\sum_{m=1}^{M} N_n[m]^2}\right)$$

   - Adding multichannel noise with a specified SNR to a multichannel signal: $\mathbf{x}[m] = \mathbf{s}[m] + \sigma\mathbf{N}[m]$

   - Note: $\sigma$ is the same for all channels (there are no different $\sigma$ per channel).

     $$SNR_{dB} = 10\log_{10}\frac{\|s\|^2}{\|\sigma N\|^2} = 10\log_{10}\frac{P_s}{\sigma^2 P_N} \;\rightarrow\; \frac{P_s}{\sigma^2 P_N} = 10^{\left(\frac{SNR_{dB}}{10}\right)} \;\rightarrow\; \sigma^2 = \frac{P_s}{P_N}\times 10^{-\left(\frac{SNR_{dB}}{10}\right)}$$

4. Extract the sources using an ICA method of your choice. You can use the function `R2COM.m` (the R2Com algorithm).
5. Examine all the obtained sources, keep the desired sources (spiky sources), and remove the other sources.
6. Project the desired sources back to the sensor domain (observation domain) and build the denoised observations (`X_den`).
7. Plot the denoised observations for channels 13 and 24 together with the original noise-free data and the noisy data.
8. Compute the RRMSE (relative RMSE) for each SNR. Analyze the obtained results.

   $$RRMSE = \frac{\sqrt{\sum_{n=1}^{32}\sum_{t=1}^{T}\left(x_{org}^{(n)}(t) - x_{den}^{(n)}(t)\right)^2}}{\sqrt{\sum_{n=1}^{32}\sum_{t=1}^{T}\left(x_{org}^{(n)}(t)\right)^2}}$$

---

## Part 2: Denoising of Real Epileptic Signals

The folder of this experiment (`Lab2_2`) contains four `.mat` files with EEG signals (`NewData1` to `NewData4`). The signals belong to epileptic patients and were recorded during seizure or non-seizure periods (sampling frequency 250 Hz). The goal is to remove the noise and artifacts of the given signals using the ICA method. Choose two of the four signals and, for each chosen signal, carry out the following steps and obtain the results.

1. Plot the signal in the time domain, labeling all channels.
2. Examine the signal for noise and artifacts. What type of artifact do you see in the signal? In your opinion, can this artifact be removed using the ICA method?
3. Apply an ICA algorithm to the signal and obtain the independent components and the mixing matrix. You can use the function `R2COM.m` (the R2Com algorithm).
4. Plot the temporal, spectral and spatial characteristics of each component and decide whether it is desirable or undesirable. To plot the spectral and spatial characteristics you can use the functions `pwelch.m` (a built-in MATLAB function) and `plottopomap.m`, respectively.

   **Note:** For the temporal and spectral characteristics use the rows of the source matrix, and for the spatial characteristic consider the corresponding columns of the mixing matrix.
5. Store the indices of all desired sources in a vector `SelSources` and reconstruct the denoised signal:

   $$X_{denoised} = A(:, SelSources) \times S(SelSources, :)$$

6. Plot the denoised signal and compare it with the original signal. Did you select the correct sources? Was the noise removal performed well? Should you repeat Step 5 more carefully?
