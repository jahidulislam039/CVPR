# CVPR Assignment: CNN Development on Custom Malware Dataset

## 📌 Project Overview
This repository contains the implementation of a Custom Convolutional Neural Network (CNN) built from scratch using PyTorch. The objective of this project is to classify malware families based purely on their 2D visual byte-plot representations. 

The model was evaluated with and without regularization techniques (Batch Normalization and Dropout) to analyze their impact on overfitting and generalization.

## ⚠️ Important Note Regarding Model Weights (`.pth`)
Due to GitHub's strict file size restriction for standard uploads (maximum 25MB), I was unable to upload the trained model weights directly to this repository, as the final `22-47144-1.pth` file is approximately **64MB**.

**To access the trained model weights:**
Please open the **`Model weight.txt`** file included in this repository. It contains a direct Google Drive link to download the `.pth` file safely.

## 📊 Dataset Origin & Extraction Strategy

### 1. The Source
The raw data for this project originates from the **MalwareVision** dataset hosted on Kaggle (created by `mohitchauhan04`). The original dataset is massive, containing over 94,000 high-resolution (1024x1024) images across 23 classes, totaling roughly 29 GB.

### 2. The "Scraping" (Extraction & Sampling) Process
To make this dataset viable for a custom CNN training loop without encountering Out-Of-Memory (OOM) errors or heavily imbalanced classes, I built a custom Python extraction pipeline to systematically sample the data:

* **Direct API Integration:** Instead of downloading the 29GB dataset manually, I used the Kaggle API directly within a Google Colab environment to mount the raw data securely.
* **Randomized Sampling:** I wrote an automated script to parse the raw directories and randomly extract exactly **500 images** from 5 distinct, visually diverse families. This guaranteed a perfectly balanced dataset, preventing the model from developing a bias toward majority classes.
* **On-the-Fly Optimization:** The extracted subset was immediately zipped and exported into a lightweight file (~2,000 images total), bypassing the need for massive local storage.

### 3. Final Dataset Specifications
Once extracted, the data was preprocessed in PyTorch to fit the custom CNN architecture:
* **Total Images:** 2,000
* **Preprocessing:** Downscaled from 1024x1024 RGB to **128x128 Grayscale** (1-channel)
* **Classes (5):**
  * `Benign`
  * `AgentTesla`
  * `LummaStealer`
  * `Mirai`
  * `SmokeLoader`
* **Data Split:** 70% Training (1,400), 15% Validation (300), 15% Testing (300)

## 🧠 Model Architecture
The network is a custom-built PyTorch CNN featuring:
* 3 Convolutional Blocks (Conv2d -> BatchNorm2d -> ReLU -> MaxPool2d)
* Fully Connected Layers with Dropout (0.5) for regularization
* Optimizer: Adam (`lr=0.001`)
* Loss Function: CrossEntropyLoss
* Learning Rate Scheduler: StepLR (`step_size=5, gamma=0.5`)

## 📈 Results & Evaluation
The model achieved a strong generalization on the test set, successfully identifying structural patterns in the malware byte-plots.
* **Overall Test Accuracy:** 89%
* **Best Performing Classes:** `Mirai` and `SmokeLoader` (F1-Score: 0.93)
* **Worst Performing Class:** `AgentTesla` (F1-Score: 0.78), which shared visual similarities with LummaStealer.
* **Overfitting Control:** The inclusion of Batch Normalization and Dropout successfully kept the training and validation loss curves tightly correlated, preventing the model from memorizing the training data. 
*(Full accuracy/loss curves and the confusion matrix are visualized inside the Jupyter Notebook).*

## 📁 Repository Structure
* `CNN_22-47144-1.ipynb`: The complete Python notebook containing data loading, preprocessing, model architecture, training loop, evaluation metrics, and analysis.
* `Model weight.txt`: Contains the Google Drive download link for the 64MB `.pth` model weights file.
* `README.md`: Project documentation.
