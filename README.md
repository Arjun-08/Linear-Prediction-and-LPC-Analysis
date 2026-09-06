# Linear Prediction and LPC Analysis

Linear Prediction is a signal processing technique for estimating a signal sample from its previous samples. In speech processing, Linear Predictive Coding (LPC) can be used to model the spectral characteristics of a speech signal.

This work applies LPC analysis to a one-second audio waveform and investigates the effect of different predictor orders:

- N = 5
- N = 10
- N = 20

For each predictor order, LPC coefficients are calculated, the prediction error is obtained, the signal is reconstructed, and the MSE is evaluated. The work also includes individual plots and comparative plots for analyzing the different cases.

The later part of the work extends the LPC analysis by recovering autocorrelation values from known LPC coefficients and MSE, implementing Levinson-Durbin recursion, analyzing its computational complexity, and comparing the Autocorrelation and Covariance methods with Levinson-Durbin.

## Methodology

The initial LPC experiment follows the workflow below:

1. Load a one-second audio clip while preserving its original sampling rate.
2. Plot the original audio waveform.
3. Compute the autocorrelation of the signal.
4. Extract the required autocorrelation values for each predictor order.
5. Solve the corresponding Toeplitz system to obtain the LPC coefficients.
6. Apply the LPC filter to obtain the prediction error.
7. Reconstruct the signal using the prediction error.
8. Calculate the MSE.
9. Repeat the analysis for N = 5, 10, and 20.
10. Generate reconstruction plots for each predictor order.
11. Generate prediction-error plots for each predictor order.
12. Generate comparison plots for prediction errors and reconstructed signals.
13. Display the LPC coefficients and MSE values for all cases.

## Input Signal

The experiment uses a WAV audio file as input.

The first one second of the audio signal is loaded while preserving its original sampling rate:

```python
x, sr = librosa.load(file_path, sr=None, duration=1.0)
```

The original waveform is plotted before performing the LPC analysis.

## Predictor Orders

The experiment considers three predictor orders:

```python
N_values = [5, 10, 20]
```

The purpose of using multiple predictor orders is to study how the number of prediction coefficients affects the accuracy of the signal model.

## LPC Coefficient Estimation

For each predictor order, the autocorrelation sequence of the input signal is calculated.

The resulting Toeplitz system is solved to obtain the LPC coefficients:

```python
a = scipy.linalg.solve_toeplitz(
    (R[:-1], R[:-1]),
    -R[1:]
)
```

The leading coefficient is then inserted to form the complete LPC coefficient vector.

The coefficients are stored separately for each value of N.

## Prediction Error

The LPC filter is applied to the input signal to obtain the prediction error:

```python
e = lfilter(a, 1, x)
```

The prediction error represents the portion of the signal that is not predicted by the LPC model.

The error signal is stored and plotted separately for each predictor order.

## Signal Reconstruction

The reconstructed signal is calculated as:

```python
reconstructed = x - e
```

The reconstructed waveform is compared with the original waveform for each predictor order.

## Mean Squared Error

The Mean Squared Error is calculated using:

```python
MSE = np.mean(e ** 2)
```

A lower MSE indicates a lower prediction error for the corresponding predictor order.

## Experimental Cases

The experiment consists of multiple cases and corresponding visualizations.

| Case | Predictor Order | Analysis |
|---|---:|---|
| Case 1 | N = 5 | LPC coefficients, prediction error, reconstruction, and MSE |
| Case 2 | N = 10 | LPC coefficients, prediction error, reconstruction, and MSE |
| Case 3 | N = 20 | LPC coefficients, prediction error, reconstruction, and MSE |
| Comparison 1 | N = 5, 10, 20 | Comparison of prediction errors |
| Comparison 2 | N = 5, 10, 20 | Comparison of reconstructed signals |

## Results

The obtained MSE values are:

| Predictor Order | MSE |
|---:|---:|
| N = 5 | 2.3122696806207264 × 10⁻⁵ |
| N = 10 | 2.060842717301354 × 10⁻⁵ |
| N = 20 | 1.5028988645095041 × 10⁻⁵ |

The MSE decreases as the predictor order increases from N = 5 to N = 20. Among the tested cases, N = 20 produces the lowest MSE.

## LPC Coefficients

### N = 5

```text
[1.0,
 -1.2747833,
 0.02351156,
 0.25724902,
 -0.06778535,
 0.12393362]
```

### N = 10

```text
[1.0000,
 -1.1680,
 -0.0217,
 0.2527,
 0.0024,
 0.0146,
 -0.1389,
 0.0150,
 0.0630,
 0.0774,
 0.0174]
```

### N = 20

```text
[1.0000,
 -1.2467,
 0.2133,
 0.0176,
 0.1107,
 0.0234,
 -0.1956,
 0.0476,
 0.0096,
 0.1809,
 -0.2032,
 0.1006,
 -0.0217,
 -0.1278,
 0.5390,
 -0.4060,
 0.2957,
 -0.4121,
 0.2380,
 -0.0512,
 -0.0176]
```

## Results and Plots

### Original Audio Waveform

The original one-second audio waveform is plotted as the reference signal.

<img width="866" height="393" alt="Original Audio Waveform" src="https://github.com/user-attachments/assets/889e4478-4982-4974-9505-7feb988c9faa" />

### Reconstructed Signal for N = 5

Comparison between the original signal and the reconstructed signal obtained using an LPC predictor order of N = 5.

<img width="866" height="393" alt="Reconstructed Signal N=5" src="https://github.com/user-attachments/assets/476add64-7ce4-48be-8dc7-4de205d144dc" />

### Reconstructed Signal for N = 10

Comparison between the original signal and the reconstructed signal obtained using an LPC predictor order of N = 10.

<img width="866" height="393" alt="Reconstructed Signal N=10" src="https://github.com/user-attachments/assets/a0b25b51-475f-47c0-b959-e408815477aa" />

### Reconstructed Signal for N = 20

Comparison between the original signal and the reconstructed signal obtained using an LPC predictor order of N = 20.

<img width="866" height="393" alt="Reconstructed Signal N=20" src="https://github.com/user-attachments/assets/4531e446-607e-46d9-b033-58822755d8fc" />

### Prediction Error for N = 5

Prediction error signal obtained using a predictor order of N = 5.

<img width="866" height="393" alt="Prediction Error N=5" src="https://github.com/user-attachments/assets/73471cf8-5c3e-4865-a3be-92d7f5c02b1a" />

### Prediction Error for N = 10

Prediction error signal obtained using a predictor order of N = 10.

<img width="866" height="393" alt="Prediction Error N=10" src="https://github.com/user-attachments/assets/52f93487-f61a-4f7a-ac73-1f02542baf81" />

### Prediction Error for N = 20

Prediction error signal obtained using a predictor order of N = 20.

<img width="866" height="393" alt="Prediction Error N=20" src="https://github.com/user-attachments/assets/478bce50-7bca-4042-9391-7cb9a09a81fb" />

### Comparison of Prediction Errors

Prediction errors for N = 5, N = 10, and N = 20 are plotted together to compare the effect of predictor order.

<img width="1021" height="547" alt="Comparison of Prediction Errors" src="https://github.com/user-attachments/assets/4e2058a5-bfe2-4737-a81f-6213dde0b40c" />

### Comparison of Reconstructed Signals

The original signal and reconstructed signals for all three predictor orders are plotted together for direct comparison.

<img width="1021" height="547" alt="Comparison of Reconstructed Signals" src="https://github.com/user-attachments/assets/4d406877-5c89-4604-9306-d73e18b9c1db" />

## Observations

The following observations are obtained from the experiment:

- Increasing the predictor order improves the approximation of the input signal.
- The MSE decreases as the predictor order increases from N = 5 to N = 20.
- N = 5 produces the highest MSE among the tested cases.
- N = 20 produces the lowest MSE among the tested cases.
- The reconstructed signals become closer to the original signal as the predictor order increases.
- The prediction-error plots provide a visual comparison of the prediction accuracy for different predictor orders.
- A higher-order LPC model provides a better representation of the analyzed signal within the tested range.
- Further increases in predictor order may eventually provide diminishing improvements.

## Results Summary

| Predictor Order | MSE | Observation |
|---:|---:|---|
| N = 5 | 2.3123 × 10⁻⁵ | Highest error among the tested cases |
| N = 10 | 2.0608 × 10⁻⁵ | Improved over N = 5 |
| N = 20 | 1.5029 × 10⁻⁵ | Lowest error among the tested cases |

The results show a reduction in MSE as the predictor order increases.

# Additional LPC Analysis

The following tasks extend the above LPC analysis by using the previously obtained LPC coefficients and MSE values, implementing Levinson-Durbin recursion, and comparing different LPC estimation methods.

## Autocorrelation from LPC Coefficients and MSE

Given the LPC coefficients and MSE values from the previous analysis, the autocorrelation sequence is recovered by solving a Toeplitz system.

The system is represented as:

$$
T R = b
$$

where:

- $T$ is the Toeplitz matrix formed using the LPC coefficients.
- $R$ represents the autocorrelation values.
- $b$ is a vector whose first element contains the MSE.

After obtaining $R(k)$, the normalized autocorrelation is calculated as:

$$
\rho(k) = \frac{R(k)}{R(0)}
$$

This provides the autocorrelation and normalized correlation values for N = 5, 10, and 20.

### Autocorrelation Results for N = 5

| k | R(k) | ρ(k) |
|---:|---:|---:|
| 0 | 0.0000189828 | 1.0000000000 |
| 1 | 0.0000018584 | 0.0979017330 |
| 2 | -0.0000164516 | -0.8666574534 |
| 3 | 0.0000077071 | 0.4060056268 |
| 4 | 0.0000139100 | 0.7327703610 |
| 5 | -0.0000195566 | -1.0302270570 |

### Autocorrelation Results for N = 10

| k | R(k) | ρ(k) |
|---:|---:|---:|
| 0 | 0.0000229588 | 1.0000000000 |
| 1 | 0.0000004006 | 0.0174470202 |
| 2 | -0.0000223137 | -0.9719007657 |
| 3 | 0.0000146795 | 0.6393836505 |
| 4 | 0.0000172921 | 0.7531781998 |
| 5 | -0.0000267514 | -1.1651913038 |
| 6 | -0.0000014685 | -0.0639631800 |
| 7 | 0.0000243747 | 1.0616704976 |
| 8 | -0.0000096066 | -0.4184262405 |
| 9 | -0.0000182429 | -0.7945908167 |
| 10 | 0.0000241265 | 1.0508613028 |

### Autocorrelation Results for N = 20

| k | R(k) | ρ(k) |
|---:|---:|---:|
| 0 | 0.0000144135 | 1.0000000000 |
| 1 | -0.0000090398 | -0.6271753767 |
| 2 | 0.0000043347 | 0.3007346969 |
| 3 | -0.0000029358 | -0.2036841460 |
| 4 | 0.0000043872 | 0.3043777903 |
| 5 | -0.0000025633 | -0.1778372336 |
| 6 | -0.0000023277 | -0.1614923991 |
| 7 | 0.0000046381 | 0.3217887042 |
| 8 | -0.0000032662 | -0.2266055266 |
| 9 | 0.0000004596 | 0.0318836150 |
| 10 | 0.0000024565 | 0.1704276108 |
| 11 | -0.0000042252 | -0.2931393730 |
| 12 | 0.0000036843 | 0.2556137050 |
| 13 | -0.0000008508 | -0.0590293899 |
| 14 | -0.0000028878 | -0.2003538486 |
| 15 | 0.0000085984 | 0.5965485851 |
| 16 | -0.0000115929 | -0.8043094095 |
| 17 | 0.0000084787 | 0.5882487430 |
| 18 | -0.0000040590 | -0.2816083528 |
| 19 | 0.0000024023 | 0.1666710395 |
| 20 | -0.0000024386 | -0.1691880189 |

## Levinson-Durbin Recursion

Levinson-Durbin recursion is implemented manually to compute LPC coefficients from the autocorrelation sequence.

The recursion iteratively estimates the LPC coefficients and prediction error.

The reflection coefficient at recursion step $k$ is calculated using the previously estimated coefficients and autocorrelation values. The prediction error is updated as:

$$
E_k = E_{k-1}(1-\lambda_k^2)
$$

The implementation is demonstrated for:

$$
N = 10
$$

The resulting LPC coefficients are then used to reconstruct the signal.

## Signal Reconstruction Using Levinson-Durbin

A normalized segment of 2048 samples is used for the Levinson-Durbin reconstruction experiment.

The predicted signal is calculated using:

$$
\hat{x}(n) =
-\sum_{j=1}^{N} a_jx(n-j)
$$

The original and reconstructed signals are plotted for comparison.

### Original vs Reconstructed Signal

<img width="993" height="528" alt="image" src="https://github.com/user-attachments/assets/8671c8c1-241f-46ab-908c-1640ff021627" />


The final Mean Squared Error obtained from the Levinson-Durbin prediction for N = 10 is:

$$
MSE = 0.2179
$$

## Prediction Error and MSE per Recursion Step

The prediction error and MSE are calculated at every Levinson-Durbin recursion step.

<img width="990" height="490" alt="image" src="https://github.com/user-attachments/assets/72d30e0a-457e-42a2-87d0-bd482e34a41f" />


The plot shows the decrease in prediction error and MSE as the recursion proceeds from step 1 to step 10.

## Computational Complexity of Levinson-Durbin

Levinson-Durbin recursion exploits the Toeplitz structure of the autocorrelation matrix to reduce the computational complexity of solving the Yule-Walker equations.

A direct matrix inversion or Gaussian-elimination-based approach has approximately:

$$
O(N^3)
$$

computational complexity.

Levinson-Durbin reduces this to:

$$
O(N^2)
$$

by iteratively updating the solution instead of solving the complete system independently at every step.

This makes Levinson-Durbin computationally efficient for LPC analysis and suitable for applications requiring efficient signal processing.

## Autocorrelation vs Covariance vs Levinson-Durbin

The Autocorrelation and Covariance methods are compared against the Levinson-Durbin recursion for:

- N = 5
- N = 10
- N = 20

For each predictor order, LPC coefficients are obtained using all three methods.

The Mean Squared Error between the coefficients obtained using each method and the Levinson-Durbin coefficients is then calculated.

### Methods Compared

1. Autocorrelation method
2. Covariance method
3. Levinson-Durbin recursion

The comparison evaluates how closely each method reproduces the LPC coefficients obtained from Levinson-Durbin recursion.

## Comparison Results

The results show that the Autocorrelation method closely matches the Levinson-Durbin solution for all three predictor orders.

The approximate MSE values between the Autocorrelation and Levinson-Durbin coefficients are on the order of $10^{-26}$.

The Covariance method produces considerably larger differences.

| Predictor Order | Autocorrelation vs LD | Covariance vs LD |
|---:|---:|---:|
| N = 5 | ≈ 10⁻²⁶ | 1.93 |
| N = 10 | ≈ 10⁻²⁶ | 1.06 |
| N = 20 | ≈ 10⁻²⁷ | 0.69 |

The calculated results indicate that the Autocorrelation method provides the closest match to the Levinson-Durbin solution.

## LPC Coefficient Comparison

The LPC coefficients obtained using the Autocorrelation, Covariance, and Levinson-Durbin methods are plotted for N = 5, 10, and 20.

<img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/339d8841-e655-40ed-859d-4c67da824dac" />


The comparison plot shows that the Autocorrelation and Levinson-Durbin coefficients closely overlap, whereas the Covariance method produces larger deviations.

## Comparison of Methods

| Method | Relationship with LD | Observation |
|---|---|---|
| Autocorrelation | Very close | Closely matches Levinson-Durbin |
| Covariance | Larger deviation | Does not match LD as closely |
| Levinson-Durbin | Reference | Efficient recursive solution |

The close agreement between the Autocorrelation method and Levinson-Durbin is expected because Levinson-Durbin solves the Yule-Walker equations associated with the Toeplitz autocorrelation matrix.

The Covariance method does not impose the same Toeplitz structure, resulting in larger differences from the Levinson-Durbin solution.


## Overall Observations

The complete analysis provides the following observations:

- Increasing the LPC predictor order reduces the MSE for the original LPC experiment.
- N = 20 gives the lowest MSE among N = 5, 10, and 20 in the initial experiment.
- Autocorrelation values can be recovered from the LPC coefficients and MSE by solving the corresponding Toeplitz system.
- The autocorrelation sequence can be normalized using R(0).
- Levinson-Durbin recursion provides an iterative approach for obtaining LPC coefficients.
- The prediction error decreases across the Levinson-Durbin recursion steps.
- The MSE also decreases as the recursion progresses.
- Levinson-Durbin reduces the computational complexity from approximately O(N³) for direct matrix-based solutions to O(N²).
- The Autocorrelation method closely matches the Levinson-Durbin solution.
- The Covariance method shows considerably larger deviations from the Levinson-Durbin coefficients.
- The results demonstrate the relationship between the Yule-Walker equations, Toeplitz structure, Autocorrelation-based LPC, and Levinson-Durbin recursion.

## Discussion

This work presents an LPC-based analysis of a speech/audio signal using different predictor orders and extends the analysis with autocorrelation recovery, Levinson-Durbin recursion, computational complexity analysis, and comparison of LPC estimation methods.

For the initial LPC analysis, predictor orders N = 5, N = 10, and N = 20 were evaluated. The MSE decreased from approximately `2.31 × 10⁻⁵` for N = 5 to approximately `1.50 × 10⁻⁵` for N = 20.

The extended analysis demonstrates how autocorrelation values can be recovered from LPC coefficients and MSE and how Levinson-Durbin can efficiently solve the corresponding prediction problem.

The comparison between the Autocorrelation, Covariance, and Levinson-Durbin methods shows that the Autocorrelation method closely matches the Levinson-Durbin solution, while the Covariance method produces larger deviations.
