# Radar Interference Mitigation Using Band-Stop Filtering

## Overview

This project demonstrates a simple **signal processing approach to remove interference from a radar signal**. A synthetic radar signal is generated and contaminated with a high-frequency interference component. A **band-stop (notch) filter** is then applied to suppress the interference, and the improvement is evaluated using **Mean Squared Error (MSE)**.

The workflow includes:

* Generating a clean radar signal
* Adding interference
* Performing frequency analysis using FFT
* Removing interference using a band-stop filter
* Evaluating signal quality before and after filtering

---

## Project Structure

```
Radar-Interference-Mitigation/
│
├── RIM.ipynb          # Main Jupyter notebook with simulation and analysis
├── README.md          # Project documentation
```

---

## Signal Model

The simulated radar signal is defined as a **10 Hz sinusoidal signal**:

```python
clean_signal = sin(2π × 10t)
```

An interference component at **50 Hz** is added:

```python
interference = 0.5 × sin(2π × 50t)
```

The final observed signal becomes:

```python
signal_with_interference = clean_signal + interference
```

---

## Frequency Analysis

To analyze the signal spectrum, the **Fast Fourier Transform (FFT)** is applied.
This helps visualize the presence of interference in the frequency domain.

Key observations:

* 10 Hz → original radar signal
* 50 Hz → interference component

---

## Interference Removal

A **Butterworth band-stop filter** is used to suppress the interference around **50 Hz**.

### Filter Parameters

| Parameter          | Value    |
| ------------------ | -------- |
| Sampling Frequency | 1000 Hz  |
| Stop Band          | 45–55 Hz |
| Filter Order       | 2        |

Example implementation:

```python
from scipy.signal import butter, filtfilt

def band_stop_filter(data, lowcut, highcut, fs, order=2):
    nyquist = 0.5 * fs
    low = lowcut / nyquist
    high = highcut / nyquist
    b, a = butter(order, [low, high], btype="bandstop")
    return filtfilt(b, a, data)
```

---

## Performance Evaluation

Signal quality is evaluated using **Mean Squared Error (MSE)**.

```python
from sklearn.metrics import mean_squared_error

mse_before = mean_squared_error(clean_signal, signal_with_interference)
mse_after  = mean_squared_error(clean_signal, filtered_signal)
```

A lower MSE after filtering indicates **successful interference suppression**.

---

## Requirements

Install the required Python libraries:

```bash
pip install numpy matplotlib scipy scikit-learn
```

Required packages:

* NumPy
* Matplotlib
* SciPy
* scikit-learn

---

## How to Run

1. Clone the repository

```bash
git clone https://github.com/yourusername/radar-interference-mitigation.git
```

2. Navigate to the project folder

```bash
cd radar-interference-mitigation
```

3. Open the notebook

```bash
jupyter notebook RIM.ipynb
```

4. Run the cells to:

* Generate signals
* Perform FFT analysis
* Apply filtering
* Evaluate performance

---

## Results

The filtering process:

* Removes the **50 Hz interference**
* Restores the original radar signal characteristics
* Reduces the **Mean Squared Error**

This demonstrates a basic **interference mitigation technique used in radar and communication systems**.

---

## Future Improvements

Possible extensions include:

* Adaptive filtering methods
* Wavelet-based interference removal
* Processing real radar data
* Multiple interference sources
* Real-time implementation

---

## License

This project is open-source and available under the **MIT License**.
