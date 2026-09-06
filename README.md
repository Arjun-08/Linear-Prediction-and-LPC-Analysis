# Linear Prediction and LPC Analysis

Linear Prediction is a signal processing technique for estimating a signal sample from its previous samples. In speech processing, Linear Predictive Coding (LPC) can be used to model the spectral characteristics of a speech signal.

This assignment applies LPC analysis to a one-second audio waveform and investigates the effect of different predictor orders:

- N = 5
- N = 10
- N = 20

For each predictor order, LPC coefficients are calculated, the prediction error is obtained, the signal is reconstructed, and the MSE is evaluated. The experiment also includes individual plots and comparative plots for analyzing the different cases.

## Methodology

The experiment follows the workflow below:

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

<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/889e4478-4982-4974-9505-7feb988c9faa" />


### Reconstructed Signal for N = 5

Comparison between the original signal and the reconstructed signal obtained using an LPC predictor order of N = 5.

<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/476add64-7ce4-48be-8dc7-4de205d144dc" />


### Reconstructed Signal for N = 10

Comparison between the original signal and the reconstructed signal obtained using an LPC predictor order of N = 10.

<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/a0b25b51-475f-47c0-b959-e408815477aa" />


### Reconstructed Signal for N = 20

Comparison between the original signal and the reconstructed signal obtained using an LPC predictor order of N = 20.

<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/4531e446-607e-46d9-b033-58822755d8fc" />


### Prediction Error for N = 5

Prediction error signal obtained using a predictor order of N = 5.

<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/73471cf8-5c3e-4865-a3be-92d7f5c02b1a" />


### Prediction Error for N = 10

Prediction error signal obtained using a predictor order of N = 10.

<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/52f93487-f61a-4f7a-ac73-1f02542baf81" />


### Prediction Error for N = 20

Prediction error signal obtained using a predictor order of N = 20.

<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/478bce50-7bca-4042-9391-7cb9a09a81fb" />


### Comparison of Prediction Errors

Prediction errors for N = 5, N = 10, and N = 20 are plotted together to compare the effect of predictor order.

<img width="1021" height="547" alt="image" src="https://github.com/user-attachments/assets/4e2058a5-bfe2-4737-a81f-6213dde0b40c" />


### Comparison of Reconstructed Signals

The original signal and reconstructed signals for all three predictor orders are plotted together for direct comparison.

<img width="1021" height="547" alt="image" src="https://github.com/user-attachments/assets/4d406877-5c89-4604-9306-d73e18b9c1db" />


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


## Discussion

This experiment demonstrates the application of Linear Prediction Coding to a speech/audio waveform and investigates the effect of predictor order on prediction accuracy and signal reconstruction.

Three predictor orders, N = 5, N = 10, and N = 20, were evaluated. The MSE decreased from approximately `2.31 × 10⁻⁵` for N = 5 to approximately `1.50 × 10⁻⁵` for N = 20.

The individual reconstruction plots, prediction-error plots, and comparison plots provide a visual analysis of the effect of predictor order.

Among the tested cases, N = 20 provides the lowest MSE and the best prediction accuracy.

