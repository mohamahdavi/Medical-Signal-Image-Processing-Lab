# Biosignal Time-Frequency Analysis: EEG, ECG, EOG and EMG

**Medical Signal and Image Processing Lab, Experiment 1 (English translation of the lab manual)**

**Original title:** Displaying EEG and ECG signals in different domains and exploring the concepts of sampling, DFT and STFT.

---

## Part 1: Electroencephalogram (EEG)

A segment of EEG is provided in the file `sig_EEG.mat`. The file contains a matrix `Z`, which is a 32-channel signal over time samples. The properties of the signal are stored in the variable `des`, which includes the sampling frequency and the channel labels (in channel order). The signal belongs to an epileptic patient and was recorded before and during a seizure. Since the signal is real, muscle artifacts are clearly visible in it.

1. Plot the signal of the 5th channel in the time domain. Set the time axis so that it is displayed in seconds. Set the figure height and width manually so that the changes of the signal shape over time are clearly visible. Write the channel label as the y-axis label or the figure title.
2. Examine the temporal-morphological features of the signal at different times (the first 15 seconds, approximately 18 to 40 seconds, approximately 45 to 50 seconds, and the final part from 50 seconds to the end of the signal). What notable features are seen in the signal?
3. Plot another channel of your choice. Is the signal shape over time the same as in the 5th channel?
4. Using the function `eeg_disp` and the code snippet below, plot the EEG signal of all channels.
   ```matlab
   offset = max(max(abs(Z)))/5;
   feq = 256;
   ElecName = des.channelnames;
   disp_eeg(Z, offset, feq, ElecName);
   ```
   For a better display, you can tune the variable `offset` (the spacing between the signals shown for two channels). Compare the behavior of the EEG signal in channel C3 (the 5th channel) with the other channels. C3 is close to the epileptic focus and clearly shows the changes of the EEG signal before and after seizure onset.
5. Figure 1 shows the EEG signal of the first six channels. Research the characteristics of the signal in the three marked segments and summarize them briefly.

   *Figure 1: A real EEG signal recorded from an epileptic patient during a seizure period (almost noise-free). Channel C3 shows seizure activity consisting of three parts: (1) rhythmic spikes before seizure onset (red box), (2) fast activity (green box), and (3) slower irregular activity (blue box).*

6. Extract the following four 5-second segments from the C3 signal:
   - Segment 1: from second 2 to 7
   - Segment 2: from second 30 to 35
   - Segment 3: from second 42 to 47
   - Segment 4: from second 50 to 55

   Plot the time signal and the frequency spectrum (DFT) of each segment and give each figure a suitable title. Time and frequency must be displayed in seconds and Hz, respectively. Adjust the frequency axes so that the frequency content is displayed well (i.e., remove the high frequencies that carry no frequency information).
7. Plot the frequency spectrum of each of the four segments using the function `pwelch.m`. Set the input parameters so that the frequency content is displayed well. What can be said about the frequency characteristics of each of the four segments?
8. Using the `spectrogram` command, with window length L = 128, overlap of N_overlap = 64 points, and number of DFT (or FFT) points nfft = L = 128, compute and plot the time-frequency spectrum of the four segments using a Hamming window (time and frequency axes in seconds and Hz, respectively). What can be said about the time-frequency characteristics of each of the four segments?
9. Consider the signal of segment 2. Since the sampling frequency is relatively high, assume that the obtained frequency spectrum is similar to the spectrum of the continuous signal. Under this assumption, and using the information obtained in Parts 6 and 7, reduce the sampling frequency (downsampling) such that the frequency content is preserved. Before reducing the sampling rate, use a suitable low-pass filter. Plot the time signal, the frequency spectrum (DFT) and the STFT of the new signal, and compare them with the results of the previous parts.

---

## Part 2: Electrocardiogram (ECG)

A segment of ECG recorded from two leads is provided in the file `sig_ECG.mat`. This signal is record number 201 of the MIT-BIH Arrhythmia database. More details about this database can be found in the file `Introduction_Arrhythmia.pdf`. The signals recorded in this database belong to healthy and diseased subjects over a duration of 30 minutes, and the R-peak locations and the label of each beat (normal/abnormal and its type) are specified.

1. Plot the signals of the two channels in time (stored in the matrix `Sig` with sampling frequency `sfreq`). The time axis must be displayed in seconds. By zooming on the horizontal axis, examine the changes of the two channels over time. Are the beats similar to each other over time in each channel? Are the beats of the two channels similar? Plot two sample (different) beats and mark the P, Q, R, S and T waves on them.
2. The occurrence time of the R wave is given in the vector `ATRTIMED` and the beat type (abnormality) in the vector `ANNOTD`. The number 1 in `ANNOTD` indicates a normal beat, and every other number indicates a type of abnormality. The abnormality corresponding to each number is described in the file `ANNOTATIONS.pdf`. For the signal of each channel, write the abnormality type of each beat below it (at the time of its R peak). You can use the `text` command.
3. Extract one or more beats of the normal type and of each abnormality, and compare their temporal-morphological features. By searching the internet, briefly explain how each type of abnormality present in this recording arises and what its characteristics are.
4. For two segments of 3 consecutive beats (one segment with normal beats and one with abnormal beats), plot the frequency spectrum and the time-frequency spectrum for each channel and compare them with each other.

---

## Part 3: Electrooculogram (EOG)

A segment of EOG recorded from two channels (left eye and right eye) is provided in the file `sig_EOG`.

1. Plot the signals of the two channels versus time (seconds) and compare them. Based on the signal shapes, sketch the approximate placement of the recording electrodes relative to the two eyes in a figure. What information can you obtain by visually inspecting the time-domain signal?
2. Plot the frequency spectrum and the time-frequency spectrum of the two channels and, based on them (and the time-domain shape of the signals), explain the characteristics of the EOG signal.

---

## Part 4: Electromyogram (EMG)

Three EMG segments recorded from three subjects using needle electrodes are provided in the file `sig_EMG.mat`. The signals were sampled at 50 kHz and the sampling frequency was then reduced to 4 kHz. One segment belongs to a healthy subject and the other two belong to two patients (Neuropathy and Myopathy). More details about the data can be found at https://archive.physionet.org/physiobank/database/emgdb.

1. Plot the time-domain signal of each of the three subjects. Zoom on the horizontal axis in different parts of the signal, examine the features of each signal, and compare them with each other.
2. Plot the frequency spectrum and the time-frequency spectrum of each subject's signal and compare them with each other.
3. By studying the two diseases, Neuropathy and Myopathy, explain the results obtained in Parts 1 and 2.
