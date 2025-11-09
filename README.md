# Unsupervised classification of bio-signals using clustering.

This repository contains the code and methodology for the research paper **"Signal and Noise Classification in Bio-Signals via Unsupervised Machine Learning"** by Sansrit Paudel, University of Rhode Island.

## Project Goal

The primary goal of this project is to develop and evaluate a pipeline that uses **unsupervised clustering algorithms** to automatically classify windowed biosignal (ECG, PPG) windows as "noisy" or "clean". It also classifies noise types like Baseline wander, motion and EMG

This research aims to find an effective method for automated data-quality assessment, which is a critical preprocessing step for most machine learning applications in healthcare.

## Methodology

The project workflow is as follows:

1.  **Load Data:** Input raw biosignals from either public sources (Physionet) or any source given available sampling rate.
2.  **Segment:** Slices the continuous signal into n-minute windows (design choice).
3.  **Feature Extraction:** Computes a feature vector for each n-minute window based on its statistical and morphological properties (see table below).
4. **Dimensionality Reduction:** Reduces the dimensionality of the feature space using PCA.
4.  **Clustering:** Feeds the resulting feature dataframe into various clustering algorithms.
5.  **Evaluation:** Compares the resulting clusters against a ground-truth labeled dataset to evaluate performance.

##  Feature Set

The features are designed to capture the **signal's shape and morphology**. This is to effectively identify noise artifacts.

| Category | Feature | Description |
| :--- | :--- | :--- |
| **Time: Amplitude** | Mean, Variance, Median | Statistics on the raw amplitude values. |
| **Time: Shape** | Skewness, Kurtosis | Describes the "peakiness" and asymmetry of the signal distribution. |
| **Time: Noise** | Zero-Crossing Rate (ZCR) | High ZCR indicates high-frequency noise (e.g., muscle artifacts). |
| **Freq: Power** | Total Power, Power (0-1 Hz) | Power in the baseline band indicates **Baseline Wander**. |
| **Freq: Power** | Power (>30 Hz) | Power in high-frequency bands indicates **Muscle Artifacts (EMG)**. |

## Models & Evaluation

This project implements and compares the following unsupervised clustering algorithms:
* **K-NN Clustering**
* **K-Means Clustering**
* **Agglomerative Clustering**
* **DBSCAN (Density-Based Spatial Clustering of Applications with Noise)**

### Evaluation

To validate the quality of the unsupervised clusters, we evaluate them against a ground-truth dataset and use standard classification metrics:

* Accuracy
* Precision
* Recall
* F1-Score
* Confusion Matrix

## Repository Structure

* `/data/`: For raw, processed, and feature data.
* `/notebooks/`: Jupyter Notebooks for exploration, prototyping, and visualization.
* `/src/`: Python scripts for the main pipeline (e.g., `build_features.py`, `train_model.py`).
* `/reports/`: For figures, tables, and final results.
* `references.bib`: Bibliography for the research paper.
* `main.tex`: The Overleaf paper.

## How to Run

1.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
2.  **Place data** in the `/data/raw/` folder.
3.  **Run the feature extraction pipeline:**
    ```bash
    python src/features/build_features.py
    ```
4.  **Run the clustering models:**
    ```bash
    python src/models/train_model.py
    ```
Alternatively, follow the step-by-step process in the Jupyter Notebooks in the `/notebooks/` directory.