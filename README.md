# sleep-stages-classification-project
classification of sleep stages with EOG signals using deep learning

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-ee4c2c)
![MESA Dataset](https://img.shields.io/badge/Dataset-MESA%20Sleep-success)

## Overview
Manual sleep stage scoring is a time-consuming, expensive process that heavily relies on trained sleep experts and is prone to inter-scorer variability. Furthermore, traditional polysomnography (PSG) involves multiple physiological signals, increasing system complexity and limiting portable monitoring use cases. 

This project tackles these challenges by developing a robust deep learning model, **SleepClass**, to accurately classify sleep stages (Wake, N1, N2, N3, REM) relying *only* on 1D EOG time-series data. 

## Dataset
We utilized the **MESA (Multi-Ethnic Study of Atherosclerosis) Sleep Dataset**.
* **Subjects:** Data from 10 selected individuals.
* **Format:** Continuous physiological signals extracted from `.edf` files, mapped with annotations from XML metadata files.
* **Epoching:** Continuous data was segmented into standard 30-second epochs to match expert annotations.

## Preprocessing Pipeline
To ensure clean and standardized input for the neural network, the following preprocessing steps were applied to the raw signals:
1. **Channel Selection:** Exclusively using the Left EOG channel.
2. **Filtering:** A bandpass filter (0.3 - 30 Hz) was applied to eliminate unwanted high/low-frequency noise.
3. **Normalization:** Z-score normalization was applied to stabilize training and improve convergence.

## Model Architecture (SleepClass)
The custom `SleepClass` architecture is a hybrid network specifically designed for 1D biomedical signals, combining spatial feature extraction with temporal sequence modeling. 
* **1D-CNN Residual Blocks:** Three sequential blocks (inspired by ResNet) with batch normalization and max pooling to extract morphological patterns while mitigating the vanishing gradient problem.
* **Bidirectional LSTM (BiLSTM):** Captures long-range temporal dependencies from both past and future contexts within the signal sequence.
* **Attention Mechanism:** A custom attention layer computes weighted sums of the hidden states, forcing the model to focus on the most salient features for stage transition.
* **Classifier:** Fully connected layers with a 0.5 Dropout rate to output the final probabilities for the 5 sleep stages.

## Experimental Iterations & Results
The architecture and training pipeline were iteratively optimized across four major versions to handle challenges like severe class imbalance and overfitting. All models were optimized using AdamW and evaluated against an unseen test set.

| Model / Notebook | Splitting Strategy | Imbalance Handling | Augmentation / Tuning | Final Test Accuracy |
| :--- | :--- | :--- | :--- | :--- |
| **V1 (`eog1.ipynb`)** | Subject-wise (7/1/2) | `WeightedRandomSampler` | Random noise, scaling | 65.95% |
| **V2 (`eog2.ipynb`)** | Random (70/15/15) | `WeightedRandomSampler` | Added Time-Shifting, Early Stopping | 74.50% |
| **V3 (`eog3.ipynb`)** | Subject-wise | **SMOTE** (Synthetic over-sampling) | Random noise, scaling | 80.25% |
| **V4 (`eog4.ipynb`)** | Random | **SMOTE** | Added **Label Smoothing (0.1)** | **82.39%** |

*Note: The best-performing model (`eog4.ipynb`) achieved 82.39% accuracy by utilizing SMOTE to generate synthetic samples for minority classes (like N1) and Label Smoothing (0.1) as a regularization technique to prevent overconfidence and overfitting.*

## Contributors
* Mennat Allah Mohamed Elsayed
* Sohaila Elsayed Ibrahim
* Wafaa Hassan Sharaan

**Supervision:** Dr. Ibrahim Sadek & Eng. Veronica William  
*Biomedical Engineering - Faculty of Engineering, Helwan University / Capital University*
