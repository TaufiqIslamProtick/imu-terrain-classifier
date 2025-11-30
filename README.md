# An Enhanced LSTM Model for Terrain Identification from Time Series Data

## Overview

This project presents an enhanced **LSTM-based time series classification** model for identifying walking terrain types using IMU (Inertial Measurement Unit) data. The model improves upon a baseline through systematic exploration of data preprocessing, windowing strategies, class imbalance handling, hyperparameter tuning, and dataset scaling.

We classify terrain into four categories:

- **0** – Solid Ground
- **1** – Down Stairs
- **2** – Up Stairs
- **3** – Grass

---

## Methodology

### **1. Data Preprocessing**

- IMU data sampled at **40 Hz** and terrain labels at **10 Hz**.
- Used **window warping** to synchronize sampling frequencies by up/down-sampling IMU signals to match label frequency.

### **2. Time-Series Windowing**

- Sequential IMU data was converted into time-series samples via **window cropping**.
- Each sample represents **w consecutive time-steps** (initially `w = 21`).
- Sliding window strategy with label taken at midpoint (`t = w/2`) of each window.

### **3. Handling Class Imbalance**

Three dataset construction strategies were explored:

1. **Choice 1:** Downsample majority classes to match the smallest class.
2. **Choice 2:** Use original data with no balancing.
3. **Choice 3:** Upsample minority classes using sampling with replacement.

### **4. Hyperparameter Tuning**

We tuned:

- **Optimizers:** SGD, RMSProp, Adam
- **Learning rate:** {0.0001, 0.0005, 0.001}
- **Dropout rate:** {0.1, 0.2, 0.3} in the LSTM layer
- **Window size w:** {21, 23, 25, 27, 29, 31}

### **5. Final Model Architecture**

- Single-layer **LSTM (100 units)**
- **ReLU** activations
- **Dropout = 0.1**
- Dense output layer with **softmax** (4 classes)
- **Adam optimizer**, **lr = 0.001**
- Implemented with **Keras (TensorFlow backend)**
- Dataset: **Upsampled balanced dataset** (Choice 3)

---

## Training & Model Selection

- Data split: **64% train**, **16% validation**, **20% separate test**.
- The best-performing configuration:

  - Optimizer: **Adam**
  - Dropout: **0.1**
  - Learning rate: **0.001**
  - Window size: **31**
  - Dataset: **Choice 3 (upsampled balanced)**

Key findings:

- Larger datasets significantly improve validation performance.
- Models trained on Choice 3 consistently outperform others.
- Dropout + tuned LR effectively eliminate overfitting.

---

## Evaluation

### **Validation Performance**

- Model shows **no overfitting**.
- Validation accuracy consistently exceeds training accuracy.

### **Separate Test Dataset (201,387 samples)**

Metrics per class:

- **Best Precision:** Class 0 (solid ground) – _99.46%_
- **Best Recall:** Class 1 (down stairs) – _100%_
- **Best F1 Score:** Class 2 (up stairs)
- **Lowest Precision:** Class 3 (grass)
- **Lowest Recall:** Class 0 (solid ground)

### **Competition Test Predictions**

- The enhanced model eliminates abrupt prediction spikes seen in the baseline.
- Produces smoother terrain transitions over time-snippet predictions.

---

## Tools & Libraries

- **Python**
- **Keras / TensorFlow**
- **scikit-learn**
- **NumPy / Pandas**
- **Matplotlib (for visualization)**
