# sleep-stages-classification-project
classification of sleep stages with EOG signals using deep learning

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-ee4c2c)
![MESA Dataset](https://img.shields.io/badge/Dataset-MESA%20Sleep-success)

##  Overview

This project presents an end-to-end deep learning solution for **automatic sleep stage classification** using only **EOG (Electrooculography)** signals. By leveraging eye movement patterns, the model effectively distinguishes between Wake, N1, N2, N3, and REM sleep stages without requiring full polysomnography (PSG) setups.

The project explores multiple model variations, data splitting strategies, and imbalance handling techniques, achieving a best **test accuracy of 82.39%** using a hybrid **1D-CNN + BiLSTM + Attention** architecture.



##  Key Features

- **EOG-only classification** — Reduces hardware complexity for potential wearable applications
- **Hybrid Deep Learning Architecture**: Residual 1D-CNN blocks + Bidirectional LSTM + Attention mechanism
- **Comprehensive preprocessing pipeline** (filtering, normalization, epoching)
- **Multiple experiments** addressing class imbalance and generalization
- **Subject-wise & random splitting** strategies evaluated
- **Data augmentation** and **SMOTE** techniques tested
- Clean, modular PyTorch implementation



## Model Architecture

The proposed **SleepClass** model consists of:

- **Residual Blocks** with 1D convolutions for robust feature extraction
- **Max Pooling** layers for dimensionality reduction
- **Bidirectional LSTM** to capture temporal dependencies
- **Custom Attention Layer** to focus on the most informative time steps
- **Fully Connected Classifier** with dropout regularization



##  Dataset

- **Source**: [MESA Sleep Dataset](https://sleepdata.org/datasets/mesa) (Multi-Ethnic Study of Atherosclerosis)
- **Subjects**: 10 individuals
- **Signals**: Left EOG channel (256 Hz sampling rate)
- **Epoch Duration**: 30 seconds
- **Classes**: Wake (0), N1 (1), N2 (2), N3 (3), REM (4)


##  RESULTS

- Notebook, Splitting,          Imbalance Handling,      Test Accuracy
- eog1,     Subject-wise,       Weighted Sampler,        65.95%
- eog2,     Sampling (70-15-15),Weighted Sampler,        74.50%
- eog3,     Subject-wise,       SMOTE,80.25%
- eog4,     samling,            SMOTE + Label Smoothing, 82.39%
