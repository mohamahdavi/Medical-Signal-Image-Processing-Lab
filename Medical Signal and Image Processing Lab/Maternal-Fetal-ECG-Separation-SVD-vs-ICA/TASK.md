# Maternal and Fetal ECG Separation: SVD vs. ICA

**Medical Signal and Image Processing Lab, Experiment 3 (English translation of the lab manual)**

**Original title:** Separation of maternal and fetal ECG signals.

---

In this experiment, you will separate the components of the cardiac signal (ECG): maternal (Figure 1 (ii)), fetal (Figure 1 (iv)) and also noise (Figure 1 (iii)), from the initial mixed signal (Figure 1 (i)) using Blind Source Separation (BSS) methods. More precisely, you will use two methods: Principal Component Analysis (using Singular Value Decomposition, SVD) and Independent Component Analysis (ICA).

*Figure 1 (from top): (i) mixture of maternal and fetal ECG, (ii) maternal ECG, (iii) noise, and (iv) fetal ECG. A 5-second window of these signals is plotted.*

The first method (SVD) uses only second-order statistics, whereas the second method (ICA) uses higher-order statistics. In this experiment you will see that taking the probability distribution of the components into account affects the performance of these two methods.

In practical applications, one of the serious challenges in evaluating source separation methods is our lack of knowledge about how the sources are generated and about their statistical properties. Therefore, in this experiment you will use synthetic ECG artifacts and a noise signal, so that the separation quality can be evaluated at the end.

---

## Part 1: Preliminary Description of the Data

The maternal ECG, fetal ECG and noise signals are given as `dat1.mecg`, `dat1.fecg` and `dat1.noise`, which you can read with the `load` command in MATLAB. Each of these files contains a vector named `recorded` in mV with sampling frequency 256 Hz.

Add these three signals together to obtain a new signal, which is a good approximation of a real signal. Answer the following questions:

1. Plot the three signals (maternal, fetal, noise) and the simulated mixed observation signal in the time domain.
2. Plot the power spectrum of these three signals using the `pwelch` command. Are the frequency contents of the maternal and fetal signals comparable? Briefly state the similarities and differences of these frequency contents.
3. Compute and report the mean and variance of these three signals. Are they comparable? Does the variance have any particular relation to the frequency content of the signals?
4. Compute the histogram of these signals using the MATLAB `hist` command (to approximate the PDF of the components) and include it in your report. Compute the 4th-order moment of these signals using the MATLAB `kurtosis` command and report it. Can you determine the degree of Gaussianity of their PDFs from these values?

---

## Part 2: Signal Separation Using Singular Value Decomposition (SVD)

One of the standard and powerful techniques for data filtering, based on second-order statistics, is SVD. In this method, the data are projected onto orthogonal axes corresponding to the highest variance of the information. These variances are learned from the data (data-driven learning), and their amounts cannot be tuned in advance. In this part, we want to analyze the variance content of the components using SVD.

1. Read the data `dat.X` and display it using the command `ch3plot`, whose file is given to you. In this data you no longer have access to the hidden components (maternal, fetal and noise signals). Instead, you have access to the mixed signal, from the three recorded channels.

   The `ch3plot` command displays two figures. In the first, the three channels are displayed in the time domain. In the second, a scatter plot of the variation of one channel against the two other channels is drawn. Note that most of the information is concentrated around the origin (which occurs for zero-mean data). The points outside this central cluster are mostly related to the QRS peak complexes.

   The goal now is to separate the fetal and maternal signals using SVD. Recall that the SVD of the initial data $\mathbf{Y} \in \mathbb{R}^{N\times M}$ can be decomposed into three matrices as $\mathbf{Y} = \mathbf{U}\mathbf{S}\mathbf{V}^T$, where $\mathbf{S} \in \mathbb{R}^{N\times M}$ is a matrix whose singular values lie on its main diagonal in descending order and all other entries are zero. The plot of these singular values is called the *eigenspectrum*. The columns of the matrix $\mathbf{V}$ are the unit eigenvectors of the matrix $\mathbf{Y}^T\mathbf{Y}$, and the matrix $\mathbf{U}$ contains the projections of $\mathbf{Y}$ onto the eigenvectors of $\mathbf{V}$. Using the MATLAB `svd` command, perform the SVD decomposition on `dat.X` and obtain the matrices $\mathbf{U}$, $\mathbf{S}$ and $\mathbf{V}$.

2. Plot the columns of the matrix $\mathbf{U}$ using the command `dv3plot`, whose file is given to you. Note the following:
   - (a) The function `dv3plot` receives the matrix $\mathbf{V}$ and the corresponding singular values as input, and uses them to perform normalization.
   - (b) Save the output figure in `.fig` format, as well as the output matrices of the SVD method.

   Recall that in the matrix $\mathbf{U} \in \mathbb{R}^{M\times M}$, which contains the projections of the initial data onto the eigenvectors of the matrix $\mathbf{V}$, only the first $N$ projections are meaningful, because the number of meaningful projections ($M$) cannot exceed the number of singular values ($N$).

3. Plot the first three columns of the matrix $\mathbf{U}$. For each of these projections, identify the dominant features (maternal, fetal, noise or mixture). Which of these projections is the most helpful for detecting the fetus? Also plot the eigenspectrum using the `stem` command and include it in your report.

   Note that this type of projection is no longer clinically meaningful. Traditional diagnosis from ECG signals is performed by checking the upper and lower bounds of discriminating thresholds in the data images. But using the SVD decomposition, we take the data into a non-clinical space in which the diagnostic thresholds clearly differ from the space of the ECG channels. Therefore, if we want to recover the clinical information in the SVD decomposition, we must bring the initial data back to the ECG sensor space using the inverse of the SVD decomposition, after applying the desired modifications. To do this, keep the singular value corresponding to the fetal component and set the other singular values to zero, thereby modifying the matrix $\mathbf{S}$, and put it into the SVD relation (using the same matrices $\mathbf{U}$ and $\mathbf{V}$) and return to the fetal sensor space.

4. Display the three recovered fetal sensor channels and describe the effect of the SVD decomposition and the modifications made. Was the recovery of the fetal signal successful?

---

## Part 3: Source Separation Using ICA

In this part, we use a technique based on higher-order statistics called Independent Component Analysis (ICA) to separate the maternal, fetal and noise signals. The mixing model can be written as "mixing matrix × sources = observations", or more precisely and mathematically as:

$$\mathbf{X}^T = \mathbf{A}\mathbf{Z}^T$$

Note that, to represent the observation matrix as time samples × channels, both sides of the relation have been transposed, where $\mathbf{X} \in \mathbb{R}^{M\times N}$, $N$ is the number of channels and $M$ is the number of time samples. Therefore, the estimation of the sources $\mathbf{Z}$ can be written as:

$$\hat{\mathbf{Z}}^T = \mathbf{W}\mathbf{X}^T$$

where the matrix $\mathbf{W}$ is the separating (unmixing) matrix and the matrix $\mathbf{W}^{-1}$ is an estimate of the mixing matrix $\mathbf{A}$.

1. Apply the ICA technique to the observation matrix ($\mathbf{X}^T$) using the function `ica`, whose file is given to you, and obtain and save the matrices $\mathbf{W}$ and $\hat{\mathbf{Z}}$. Then also compute and save the matrix $\mathbf{W}^{-1}$, which is an estimate of the mixing matrix $\mathbf{A}$.
2. Plot the scatter plot of the initial data using the function `ch3plot`, and of the three sources (components) using `dv3plot`. Note that the function `dv3plot` takes the columns of the matrix $\mathbf{W}^{-1}$ as input and can be called multiple times. Save the obtained figure in `.fig` format.
3. Plot the three columns of the matrix $\mathbf{W}^{-1}$, or equivalently, the three rows of the matrix $\hat{\mathbf{Z}}^T$. Keep the column of $\mathbf{W}^{-1}$ that, in your opinion, best represents the content of the fetal component more than the other columns, and set the other columns to zero. Using this modified $\mathbf{W}^{-1}$ and the matrix $\hat{\mathbf{Z}}$, bring the fetal component back to the ECG sensor space.
4. Plot and save the data recovered with ICA. In your opinion, was the fetal component recovered successfully?

---

## Part 4: Comparisons

1. Plot the following in a single figure in 3D space (corresponding to the three channels):
   - The scatter plot of the observation matrix $\mathbf{X}$.
   - The scatter plot of the recovered observation matrix in the two Parts 2 and 3.
   - The directions of the columns of the matrix $\mathbf{V}$.
   - The directions of the columns of the matrix $\mathbf{W}^{-1}$.

   Compare the plots with each other and analyze the results obtained from the two methods. Compute the angle between the different directions using the `dot` command. Also compute and compare the norms of the different axes.

   The fundamental question is how successful these two methods have been in separation. The only way to answer this is for our knowledge of the sources and noise to be complete. Therefore, since simulated data are used here, our knowledge of the desired signal (fECG) and the undesired signal (noise + mECG) is complete. Regarding the initial data matrix $\mathbf{X}$, the desired signal fECG is given in `dat2.fecg`, which is a scaled version of the data `dat1.fecg`.

2. By plotting the reconstructed desired signal obtained from the SVD and ICA methods (one of the channels of the signal reconstructed by these two methods) and the ideal desired signal `dat2.fecg`, compare their relative performance and determine the better method.
3. Then compute the correlation coefficient between the ideal signal `dat2.fecg` and the signal reconstructed by these two methods (one of the channels of the signal reconstructed by these two methods) using the MATLAB `corrcoef` command, and carry out the comparison.
4. Make a final comparison between the SVD and ICA methods and state the advantages and disadvantages of these two methods in general.
5. State the most important point that you learned from this experiment.
