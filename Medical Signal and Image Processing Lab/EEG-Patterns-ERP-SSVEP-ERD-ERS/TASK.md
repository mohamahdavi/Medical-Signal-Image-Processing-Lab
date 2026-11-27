# EEG Signal Patterns and Their Detection Methods: P300 ERP, SSVEP and ERD/ERS

**Medical Signal and Image Processing Lab, Experiment 4 (English translation of the lab manual)**

**Original title:** EEG signal patterns and methods for detecting them.

---

## Part 1: Event-Related Potential (ERP)

The EEG signal (ERP_EEG) stored in `EEG_ERP.mat` contains EEG recorded from one channel (Pz). The brain signal was recorded in response to a visual stimulus and contains the P300 event-related potential and background EEG. 2550 trials were performed and the brain signal was recorded in response to the stimulus. The sampling frequency is 240 Hz, and in each trial the signal was recorded from the stimulus onset until 1 second after it.

Assuming that the latency of the brain response to the stimulus is the same in all trials, we want to use synchronous averaging to determine the minimum number of trials needed to extract the P300 response from them.

a) For N = 100:100:2500, plot the average response obtained from N patterns. Plot the graphs in a single figure and compare them as N increases.

b) For N = 1:2550, plot the maximum absolute amplitude of the signal versus the number of averaged patterns in a single graph.

c) Plot the root mean square (RMS) error between the N-th average pattern and the (N−1)-th average pattern versus the number of averaged patterns N = 1:2550 in a single graph.

d) Using the results obtained in parts (a), (b) and (c), what is the required number of trials from which the P300 response can be extracted? (Call the obtained number N₀.)

e) In a single graph, plot the average response obtained in part (d) together with the following average responses, and compare them. Examine and analyze the obtained results.
   - The average response for N = 2550
   - The average response for N = N₀/3
   - The average response for N = N₀ with N₀ randomly chosen responses out of the 2550 responses
   - The average response for N = N₀/3 with N₀/3 randomly chosen responses out of the 2550 responses

f) Investigate a few real experiments based on the P300 signal. In real experiments that use P300 patterns (such as P300-based brain-computer interfaces), how many repetitions of the P300 pattern are used? Is this number consistent with the results obtained in the previous parts? Explain the reason for this difference.

---

## Part 2: Steady-State Visual Evoked Potential (SSVEP)

The file `SSVEP_EEG.mat` contains the SSVEP signal recorded from one user. This file contains a matrix `SSVEP_Signal` (EEG recorded from 6 channels Pz, Oz, P7, P8, O1 and O2, with sampling frequency 250 Hz), a matrix `Events` (stimulation frequencies) and a matrix `Event_Samples` (the time sample corresponding to the start of each stimulus). The goal is to determine the stimulation frequency from the EEG signal recorded in each trial.

a) For each channel of the data, use a band-pass filter to remove frequencies below 1 Hz and above 40 Hz. For each channel, plot the denoised signal together with the original signal in one graph.

b) Extract the 15 trials corresponding to the 15 stimuli, each with a 5-second window.

c) For each trial, compute the frequency content of all six channels and plot them in one figure. You can use the `pwelch` function. Use an appropriate legend to indicate which channel each curve belongs to.

d) For a single trial, do all channels have the same frequency content? What is the reason for the differences in the frequency content of the channels?

e) For each trial, can you determine the dominant frequency using the frequency content of the channels? To which frequencies do the spectral peaks correspond? Why does each of these peaks arise?

f) Do you know another method for determining the dominant frequency in the SSVEP signal that gives better results than using the frequency content of the individual channels? By searching the internet and relevant papers, name other methods for processing SSVEP signals and determining the dominant frequency.

---

## Part 3: Event-Related Synchronization/Desynchronization (ERS/ERD)

The file `FiveClass_EEG.mat` contains an EEG signal recorded at a sampling frequency of 256 Hz from one user while performing different mental tasks. The EEG signal was recorded from the following 30 channels (international 10-20 system):

AFz, F7, F3, Fz, F4, F8, FC3, FCz, FC4, T3, C3, Cz, C4, T4, CP3, CPz, CP4, P7, P5, P3, P1, Pz, P2, P4, P6, P8, PO3, PO4, O1 and O2.

Five mental tasks were assigned to the person:

1. **Word generation:** forming words in the mind (the words must start with the letter shown on the screen).
2. **Mental subtraction:** two numbers are shown on the screen; the person must subtract the smaller number from the larger one and repeat this. Example: 120 − 7 = 113, 113 − 7 = 106, and so on.
3. **Navigation:** the person must imagine themselves in a place such as a house and imagine the path to a specific place in the house.
4. **Hand movement imagery.**
5. **Foot movement imagery.**

In performing each mental task, first (at time zero) a "+" sign is shown on the screen (for concentration and readiness for the experiment), and after 3 seconds a beep is played and the person's task is shown on the screen. The subject must perform this task for 7 seconds (until the second beep is played). Between every two trials there is a rest of 2.5 to 3.5 seconds. The timing diagram of the experiment is shown in the figure of the manual (a 4 s lead-in, then trials T1, T2, ..., TN, then 4 s; within each trial: cross at 0 s, cue and beep at 3 s, second beep at 10 s, followed by an inter-trial interval (ITI) of 2.5 to 3.5 s).

The experiment data consist of several matrices as follows:

- `X`: the recorded EEG matrix of size (time samples × channels), containing 200 repetitions of the experiment.
- `y`: the class label of each of the 200 trials.
- `trial`: the time-sample index of the start of each of the 200 trials (corresponding to the moment the "+" is shown at the beginning of the trial).

In this experiment, we want to examine the changes of the signal in different frequency bands in each of the five different classes.

a) First, filter the signal of each channel in the following four bands: delta (1 to 4 Hz), theta (4 to 8 Hz), alpha (8 to 13 Hz) and beta (13 to 30 Hz). For this purpose, you can use the filter below (for example, for the alpha band):

```matlab
% Bandpass filter between 8 - 13
fs = 256;      % Sampling Frequency
N = 4;         % Order
Fpass1 = 8;    % First Passband Frequency
Fpass2 = 13;   % Second Passband Frequency
Apass = 1;     % Passband Ripple (dB)
% Construct an FDESIGN object and call its CHEBY1 method.
h = fdesign.bandpass('N,Fp1,Fp2,Ap', N, Fpass1, Fpass2, Apass, fs);
Hd = design(h, 'cheby1');
for c = 1:30
    Alpha_X(:,c) = filter(Hd, X(:,c));
end
```

To verify that the filters work correctly, plot about 5 seconds of the first channel of X, the original signal, and the four signals obtained in the different bands.

b) Extract the 10-second trials from the obtained signals (using the information in the vector `trial`). Example:

```matlab
for i = 1:200
    Alpha_Trials(:,:,i) = Alpha_X(trial(i):trial(i)+256*10, :);
end
```

c) To compute the power at each point, compute the square of the amplitude of all time points for each trial in each band and each channel.

d) According to the labels of the 200 trials (in the vector `y`), separate the data of the 5 different classes and, for each of the bands, obtain the average of each channel over the trials of that class (the results of part (c)). The output of this step will be four tensors with the following sizes:

- `Delta_X_avg` ∈ ℝ^(2560×30×5)
- `Theta_X_avg` ∈ ℝ^(2560×30×5)
- `Alpha_X_avg` ∈ ℝ^(2560×30×5)
- `Beta_X_avg` ∈ ℝ^(2560×30×5)

where 30 is the number of channels and 5 is the number of classes.

e) To smooth the variations in the average signals of part (d), filter the signal of each channel, in each trial and in each band, using a rectangular window defined as `newWin = ones(1,200)/sqrt(200)` with the `conv` command.

f) Select one channel (preferably a channel from the central regions of the head, such as CPz) and plot the results of part (e) for it as follows:
   - For each frequency band, plot the time-averaged signal (over the 10-second interval) of all 5 classes in one figure (four figures in total, with 5 curves in each figure).

g) Considering that the mental tasks start at approximately the third second, by comparing the graphs obtained in part (f), what conclusions can be drawn about the frequency information of the five classes?

---

### Translator's notes (ambiguities in the original manual)

- Part 1(d): the original text defines the notation N₀ for the answer; "N₀/3" in part (e) is one third of that number.
- Part 2: the original lists the channels in the order Pz, Oz, P7, P8, O1, O2.
- Part 3: the original manual numbers the last two items both as "(و)" (f); the final question is therefore effectively item (g).
- Part 3(b) extracts `trial(i):trial(i)+256*10`, which is 2561 samples; the tensors in part (d) are stated as 2560 samples in the manual.
