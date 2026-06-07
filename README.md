
# CNN-Based Image Classification Project

## Overview

This repository contains the implementation of a Convolutional Neural Network (CNN) for image classification, developed as part of the Phase 2 project requirements. The project includes data preprocessing, model training, evaluation, visualization, and comparison with a transfer learning model.

---

## Project Structure

```text
├── notebook_fixed_improved.ipynb   # Main project notebook
├── README.md                       # Project documentation
├── requirements.txt                # Required Python packages
├── outputs/                        # in progress
└── dataset/                        # Dataset directory (not uploaded to GitHub)
```

---

## Requirements

### Software

* Python 3.9+
* Jupyter Notebook or Kaggle Notebook
* Git

### Python Libraries

```bash
tensorflow
keras
numpy
pandas
matplotlib
seaborn
scikit-learn
opencv-python
pillow
```

Install all dependencies:

```bash
pip install -r requirements.txt
```

---

## Dataset Setup

### Option 1: Kaggle Dataset

1. Download the dataset from:

```text
[INSERT DATASET LINK]
```

2. Add the dataset to your Kaggle notebook.

3. Update the dataset path if necessary.

Example:

```python
dataset_path = "/kaggle/input/dataset-name"
```

---

### Option 2: Local Machine

Create the following directory structure:

```text
dataset/
├── class_1/
├── class_2/
├── class_3/
└── ...
```

Update the dataset path inside the notebook:

```python
dataset_path = "dataset/"
```

---

## Running the Project

### Using Kaggle

1. Upload `notebook_fixed_improved.ipynb`
2. Attach the dataset
3. Run all notebook cells

---

### Using Jupyter Notebook

Start Jupyter:

```bash
jupyter notebook
```

Open:

```text
notebook_fixed_improved.ipynb
```

Run all cells sequentially.

---

## Project Workflow

The notebook performs the following tasks:

### 1. Dataset Loading

* Load images from dataset directory
* Read class labels

### 2. Data Preprocessing

* Image resizing
* Image normalization
* Label encoding

### 3. Data Splitting

* Training Set
* Validation Set
* Test Set

### 4. Data Augmentation

* Rotation
* Zoom
* Horizontal flip
* Width and height shifts

### 5. CNN Model Development

* Convolution layers
* Pooling layers
* Dense layers
* Dropout regularization

### 6. Transfer Learning

Comparison with a pretrained model such as:

* MobileNetV2
* ResNet50
* EfficientNetB0

### 7. Model Training

* Early stopping
* Model checkpointing
* Performance monitoring

### 8. Model Evaluation

* Accuracy
* Precision
* Recall
* F1-score
* Classification report

### 9. Visualization

* Accuracy curves
* Loss curves
* Confusion matrix
* ROC curves (optional)
* Grad-CAM visualizations (optional)

---

## Output Files

The notebook generates:

| File                      | Description                      |
| ------------------------- | -------------------------------- |
| accuracy_curve.png        | Training and validation accuracy |
| loss_curve.png            | Training and validation loss     |
| confusion_matrix.png      | Confusion matrix visualization   |
| classification_report.txt | Evaluation metrics               |
| cnn_model.h5              | Trained CNN model                |
| transfer_model.h5         | Trained transfer learning model  |

---

## Reproducibility

To reproduce the results:

1. Download the dataset.
2. Install all dependencies.
3. Update dataset paths if required.
4. Run the notebook from start to finish.
5. Generated figures and metrics will be saved automatically.

---

## Results

| Model             | Accuracy | Precision | Recall | F1 Score |
| ----------------- | -------- | --------- | ------ | -------- |
| Custom CNN        | XX.XX%   | XX.XX%    | XX.XX% | XX.XX%   |
| Transfer Learning | XX.XX%   | XX.XX%    | XX.XX% | XX.XX%   |

Replace the values above with your final experimental results.

---

## Author

Siddhant Santosh Mathapati

Phase 2 – Proposal and Code Implementation

Deep Learning and Computer Vision Project
