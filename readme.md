# EuroSAT Multispectral Enhancements: Highway vs. River

This repository contains a Jupyter Notebook that builds and compares two land-cover classifiers on the EuroSAT dataset. The primary goal is to improve the model's ability to distinguish between highways and rivers, a known confusion point identified in the original EuroSAT benchmarks.

## Overview

The EuroSAT dataset consists of 27,000 labeled, geo-referenced Sentinel-2 images categorized into 10 distinct land use and land cover classes. While standard RGB models achieve high overall accuracy, they can struggle with specific class pairs with similar visual textures, such as highways and rivers. 

This project bridges that gap by moving beyond standard RGB training. It leverages the full 13 spectral bands captured by the Sentinel-2 satellite and incorporates calculated environmental indices to significantly enhance class separability.

## Methodology

This pipeline implements a custom data loader and adapts state-of-the-art CNNs to process deep multispectral satellite data:

* **Custom Data Pipeline:** Utilizes a custom PyTorch dataset class to read Sentinel-2 GeoTIFF images containing all 13 original spectral bands. 
* **Normalization & Feature Engineering:** Reflectance values are normalized for stable training. The pipeline then computes the Normalized Difference Vegetation Index (NDVI) and Normalized Difference Water Index (NDWI), appending them to the input tensor to create a robust 15-channel input.
* **Multispectral Model (Proposed):** Adapts the input layer of a pretrained ResNet50 to accept 15-channel data, fine-tuning the network over 5 epochs.
* **RGB Baseline Model:** Trains a standard pretrained ResNet18 utilizing strictly the first 3 channels (RGB) to serve as a direct performance baseline.

## Evaluation & Metrics

The notebook includes a detailed analytical breakdown to demonstrate how multispectral features improve upon RGB-only training. Evaluation outputs include:

* **Targeted Reports:** Specific classification reports isolating Highway vs. River performance.
* **Pairwise Confusion Matrices:** Focused confusion matrices highlighting the reduction in false positives/negatives between the Highway and River classes.
* **Global Confusion Matrices:** Full multiclass confusion matrices for all 10 land cover classes.
* **Overall Accuracy:** Complete dataset classification reports and global accuracy metrics comparing the 15-channel ResNet50 against the RGB ResNet18.

## Getting Started

### Prerequisites
* Python 3.8+
* PyTorch & Torchvision
* Pandas, NumPy, Scikit-learn
* Rasterio (for reading GeoTIFFs)
* Matplotlib & Seaborn (for metric visualizations)

### Data Preparation
1. Download the EuroSAT multispectral (GeoTIFF) dataset.
2. Ensure your train, validation, and test CSV splits are located in the `data/` directory.

### Running the Code
Run all cells in the provided Jupyter Notebook to install dependencies, load the label metadata, train both models concurrently, and generate the comparative analysis reports.

### RGB MODEL

| Class | Precision | Recall | F1-Score | Support |
| :--- | :---: | :---: | :---: | :---: |
| **Highway** | 0.96 | 0.97 | 0.96 | 250 |
| **River** | 0.98 | 0.92 | 0.95 | 250 |
| | | | | |
| **micro avg** | 0.97 | 0.94 | 0.96 | 500 |
| **macro avg** | 0.97 | 0.94 | 0.96 | 500 |
| **weighted avg** | 0.97 | 0.94 | 0.96 | 500 |

<br>

### MULTISPECTRAL MODEL

| Class | Precision | Recall | F1-Score | Support |
| :--- | :---: | :---: | :---: | :---: |
| **Highway** | 0.99 | 0.98 | 0.98 | 250 |
| **River** | 0.99 | 0.99 | 0.99 | 250 |
| | | | | |
| **micro avg** | 0.99 | 0.98 | 0.99 | 500 |
| **macro avg** | 0.99 | 0.98 | 0.99 | 500 |
| **weighted avg** | 0.99 | 0.98 | 0.99 | 500 |
