# Project Log-book

## Project Title

**Design and Implementation of an AI-Assisted Mobile Melanoma Screening System Based on Multi-Modal Fusion**

## Student Information

| Item              | Details |
| ----------------- | ------- |
| Student Name      | Pengcheng Sun |
| Project Title     | Design and Implementation of an AI-Assisted Mobile Melanoma Screening System Based on Multi-Modal Fusion |
| Dataset           | ISIC2020 |
| Main Model        | ResNet50 |
| Baseline Model    | Image-only ResNet50 |
| Proposed Model    | ResNet50 image branch + clinical metadata branch |
| Fusion Strategy   | Feature-level fusion |
| Application       | Flutter mobile application + FastAPI backend |
| Supervisor        | Omid Pournik |
| Academic Year     | 2025-2026 |

------



# 1. Project Overview

## 1.1 Project Aim

The aim of this project is to design and implement an AI-assisted mobile melanoma screening system based on multi-modal fusion. The project uses the ISIC2020 dataset to train and evaluate a baseline image-only model and a multi-modal model that combines dermoscopic images with clinical metadata.

The project focuses on both model performance and system implementation. The image-only ResNet50 model is used as the baseline, while the multi-modal model introduces clinical inputs such as age, sex and anatomical site. The final application allows users to upload or capture a lesion image, enter clinical information, run the trained model through a backend service, and view prediction results and history.

## 1.2 Engineering Context

This project combines medical image classification, clinical metadata processing, deep learning model training, model evaluation, backend inference and mobile application development. The system is designed as an auxiliary screening tool and must not be used as a replacement for professional medical diagnosis.

## 1.3 Main Deliverables

- ISIC2020 dataset preparation and preprocessing
- Image-only ResNet50 baseline model
- Clinical metadata preprocessing pipeline
- Multi-modal fusion model
- Model evaluation using Accuracy, AUC, Average Precision, Recall, Specificity, F1-score and confusion matrix
- Training curves and detailed visual analysis for dissertation evidence
- FastAPI backend for PyTorch model inference
- Flutter mobile application for image upload, clinical input, prediction display and history records
- Final report and presentation

------



# 2. Weekly Project Log

## Week 1 — Project Understanding and Initial Research

**Date:** **2026.5.18-2026.5.24**

### Work Completed

- Reviewed the project topic and clarified the project scope.
- Studied the melanoma screening problem and the role of AI-assisted medical image analysis.
- Compared potential datasets such as HAM10000, ISIC2019 and ISIC2020.
- Decided to use ISIC2020 because it contains dermoscopic images and clinical metadata.

### Dataset Comparison

| Dataset  | Main Features | Limitation | Decision |
| -------- | ------------- | ---------- | -------- |
| HAM10000 | Classic skin lesion image dataset, suitable for basic image classification. | Smaller dataset and limited clinical metadata. | Not selected as the main dataset. |
| ISIC2019 | Larger skin lesion image dataset with multiple disease categories. | Less suitable for this project's metadata fusion direction. | Used as a reference dataset only. |
| ISIC2020 | Contains dermoscopic images and clinical metadata such as age, sex and anatomical site. | Strong class imbalance between benign and melanoma cases. | Selected as the main dataset. |

### Technical Decisions

ISIC2020 was selected as the main dataset because it supports both image-based melanoma classification and clinical metadata fusion. This matches the project aim of developing a multi-modal mobile melanoma screening system.

### Problems Encountered

- The dataset is highly imbalanced, with far fewer malignant samples than benign samples.
- It was necessary to define a clear baseline before evaluating whether multi-modal fusion improves performance.

### Actions / Solutions

- Planned to evaluate the model using AUC, Average Precision, Recall, F1-score, balanced accuracy and confusion matrix instead of relying only on accuracy.
- Planned an image-only baseline and a multi-modal fusion model for fair comparison.

### Next Steps

- Download and inspect ISIC2020 data.
- Build the initial data preprocessing pipeline.

------

## Week 2 — Dataset Preparation

**Date:** **2026.5.25-2026.5.31**

### Work Completed

- Downloaded and prepared the ISIC2020 dataset.
- Organised images into benign and malignant classes.
- Inspected the clinical metadata file.
- Identified useful metadata fields:
  - `age_approx`
  - `sex`
  - `anatom_site_general_challenge`
  - `target`
- Checked image paths and label distribution.

### Prepared Dataset Summary

| Class | Number of Images |
| ----- | ---------------- |
| Benign | 32,542 |
| Malignant | 584 |
| Total | 33,126 |

### Technical Decisions

| Decision | Reason |
| -------- | ------ |
| Resize images to 224 x 224 | Standard input size for ResNet50 and suitable for baseline comparison. |
| Normalize images using ImageNet statistics | Matches the pretrained ResNet50 preprocessing convention. |
| Encode sex and anatomical site using one-hot encoding | Converts categorical clinical data into numerical model inputs. |
| Normalize age | Keeps numerical metadata within a stable range. |
| Use unknown category for missing clinical values | Allows incomplete metadata to be processed consistently. |

### Problems Encountered

- Some clinical metadata values were missing.
- The dataset has severe class imbalance.

### Actions / Solutions

- Used default or unknown values for missing clinical fields.
- Used stratified train/validation splitting to preserve the class ratio.
- Planned to report melanoma-focused metrics rather than accuracy alone.

### Next Steps

- Implement the PyTorch dataset class.
- Prepare the baseline training script.

------

## Week 3 — Baseline Model Design and Training

**Date:** **2026.6.01-2026.6.7**

### Work Completed

- Defined the baseline as an image-only ResNet50 model.
- Implemented the training pipeline.
- Added validation monitoring and history logging.
- Saved training history and evaluation results for dissertation figures.

### Baseline Model Structure

```text
Dermoscopic Image
      ↓
ResNet50
      ↓
Image Feature
      ↓
Classifier
      ↓
Benign / Malignant Prediction
```

### Technical Decisions

| Decision | Reason |
| -------- | ------ |
| Use ResNet50 as baseline | It is a stable and widely used CNN backbone. |
| Use image-only input for baseline | It provides a fair comparison for the later multi-modal model. |
| Use validation-set evaluation | Prevents reporting inflated performance from training data. |
| Save training curves and confusion matrix | Provides visual evidence for the dissertation. |

### Problems Encountered

- Training on Mac with MPS was slower than expected.
- The dataset imbalance made accuracy misleading.
- Some previous output folders contained incomplete or invalid experiments.

### Actions / Solutions

- Used MPS device on Mac and set `num_workers=0` for compatibility.
- Cleaned invalid output files and kept only valid experiment results.
- Compared models on the same validation split.

### Next Steps

- Train the multi-modal model using image and clinical metadata.
- Compare the multi-modal model against the image-only baseline.

------

## Week 4 — Multi-Modal Fusion Model Design

**Date:** **2026.6.08-2026.6.14**

### Work Completed

- Designed the multi-modal neural network architecture.
- Selected feature-level fusion as the fusion strategy.
- Implemented an MLP branch for clinical metadata.
- Combined image features and clinical metadata features before final classification.

### Multi-Modal Network Structure

```text
Dermoscopic Image → ResNet50 → Image Feature

Clinical Metadata → MLP → Clinical Feature

Image Feature + Clinical Feature
      ↓
Concatenation
      ↓
Classifier
      ↓
Benign / Malignant Prediction
```

### Fusion Strategy

The selected fusion strategy is feature-level fusion. The model first extracts high-level image features from ResNet50 and clinical features from an MLP branch. These features are concatenated and passed to the final classifier.

### Technical Decisions

| Decision | Reason |
| -------- | ------ |
| Use feature-level fusion | Combines image and tabular features before final prediction. |
| Use MLP for clinical metadata | Clinical fields are structured tabular data. |
| Use concatenation | Simple, interpretable and easy to implement. |
| Keep the same image size and validation split as baseline | Ensures a fair comparison. |

### Problems Encountered

- The clinical metadata branch needed to be lightweight to reduce overfitting.
- The frontend anatomical site options needed to match the metadata categories used by the model.

### Actions / Solutions

- Used a compact MLP metadata branch.
- Standardised clinical input values such as age, sex and anatomical site.
- Added clinical input fields to the mobile application.

### Next Steps

- Train the formal multi-modal model.
- Generate detailed model evaluation figures.

------

## Week 5 — Model Evaluation and Visual Analysis

**Date:** **2026.6.15-2026.6.21**

### Work Completed

- Trained the formal image-only baseline model.
- Trained the formal multi-modal fusion model.
- Evaluated both models on the same validation set.
- Generated training curves, metric summaries, confusion matrices, ROC curves, PR curves, probability distributions and threshold analysis figures.
- Removed invalid or duplicated result folders to avoid mixing incomplete experiments with formal results.

### Formal Experiment Settings

| Item | Baseline | Multi-Modal |
| ---- | -------- | ----------- |
| Dataset | ISIC2020 | ISIC2020 |
| Validation samples | 6,625 | 6,625 |
| Image size | 224 x 224 | 224 x 224 |
| Epochs | 8 | 8 |
| Batch size | 32 | 32 |
| Learning rate | 1e-4 | 1e-4 |
| Device | MPS | MPS |
| Random seed | 42 | 42 |

### Validation Results

| Metric | Image-only Baseline | Multi-Modal Model |
| ------ | ------------------- | ----------------- |
| Accuracy | 0.9020 | 0.9197 |
| AUC | 0.8550 | 0.8742 |
| Average Precision | 0.1277 | 0.1519 |
| Malignant Recall | 0.5128 | 0.5385 |
| Specificity | 0.9090 | 0.9266 |
| Malignant Precision | 0.0920 | 0.1165 |
| Malignant F1-score | 0.1560 | 0.1915 |
| Balanced Accuracy | 0.7109 | 0.7325 |

### Confusion Matrix Results

| Model | TN | FP | FN | TP |
| ----- | -- | -- | -- | -- |
| Image-only Baseline | 5916 | 592 | 57 | 60 |
| Multi-Modal Model | 6030 | 478 | 54 | 63 |

### Observations

- The multi-modal model outperformed the image-only baseline on AUC, Average Precision, malignant recall, specificity, F1-score and balanced accuracy.
- Accuracy alone is not sufficient because the dataset is strongly imbalanced.
- The multi-modal model reduced false positives and slightly reduced false negatives compared with the baseline.
- The improvement supports the project hypothesis that clinical metadata can provide useful supplementary information for melanoma screening.

### Next Steps

- Integrate the best multi-modal model into the backend.
- Test model inference through the mobile application.

------

## Week 6 — Backend Inference and Mobile Application Integration

**Date:** **2026.6.22-2026.6.28**

### Work Completed

- Integrated the trained PyTorch model with a FastAPI backend.
- Implemented image upload and clinical metadata input for prediction.
- Added user authentication.
- Added prediction history storage in SQLite.
- Built Flutter screens for learning content, lesion checking and history records.
- Tested predictions using selected ISIC2020 sample images.

### System Workflow

```text
Flutter App
   ↓
Image + Clinical Inputs
   ↓
FastAPI Backend
   ↓
PyTorch Multi-Modal Model
   ↓
Prediction Result
   ↓
Result Display + History Record
```

### Technical Decisions

| Decision | Reason |
| -------- | ------ |
| Use FastAPI backend for inference | Keeps PyTorch preprocessing and model execution consistent with training. |
| Store prediction history in SQLite | Provides persistent user records for the mobile app. |
| Use backend inference instead of local ONNX inference | More stable for the current project and easier to verify. |
| Present output as screening risk | Avoids claiming final medical diagnosis. |

### Problems Encountered

- Prediction results were initially not saved to the database.
- Some app text looked like a demo rather than a real app.
- The bottom navigation and login UI needed improvement.

### Actions / Solutions

- Added a `prediction_history` table and history API endpoint.
- Added a history screen in the Flutter app.
- Reworked the home page, login page and bottom navigation into a more realistic mobile app style.
- Removed unnecessary demo-style descriptions from the frontend.

### Next Steps

- Finalise the application UI.
- Use formal experiment results in the dissertation.

------

# 3. Experiment Log

## Experiment 1 — Image-Only ResNet50 Baseline

**Date:** **2026.6.15-2026.6.21**

| Item | Details |
| ---- | ------- |
| Dataset | ISIC2020 |
| Input | Dermoscopic image only |
| Image Size | 224 x 224 |
| Model | ResNet50 |
| Epochs | 8 |
| Batch Size | 32 |
| Learning Rate | 1e-4 |
| Device | MPS |
| Validation Samples | 6,625 |

### Results

| Metric | Value |
| ------ | ----- |
| Accuracy | 0.9020 |
| AUC | 0.8550 |
| Average Precision | 0.1277 |
| Malignant Recall | 0.5128 |
| Specificity | 0.9090 |
| Malignant Precision | 0.0920 |
| Malignant F1-score | 0.1560 |
| Balanced Accuracy | 0.7109 |

### Observations

- The baseline provides a clear image-only comparison point.
- The model achieves high overall accuracy, but malignant precision and F1-score are limited because of class imbalance.
- This confirms the need to evaluate medical classification models using more than accuracy.

### Evidence

Formal output directory:

```text
/Users/spc/skin_app/backend/outputs/baseline_resnet50_image_only_224_8ep
```

Generated evidence includes:

- `history.csv`
- `metrics_summary.csv`
- `predictions.csv`
- training curves
- confusion matrix
- ROC curve
- PR curve
- probability distribution
- threshold analysis

------

## Experiment 2 — Multi-Modal Fusion Model

**Date:** **2026.6.15-2026.6.21**

| Item | Details |
| ---- | ------- |
| Dataset | ISIC2020 |
| Inputs | Image + age + sex + anatomical site |
| Image Branch | ResNet50 |
| Metadata Branch | MLP |
| Fusion Method | Feature-level fusion |
| Epochs | 8 |
| Batch Size | 32 |
| Learning Rate | 1e-4 |
| Device | MPS |
| Validation Samples | 6,625 |

### Results

| Metric | Image-only Baseline | Multi-Modal Model |
| ------ | ------------------- | ----------------- |
| Accuracy | 0.9020 | 0.9197 |
| AUC | 0.8550 | 0.8742 |
| Average Precision | 0.1277 | 0.1519 |
| Malignant Recall | 0.5128 | 0.5385 |
| Specificity | 0.9090 | 0.9266 |
| Malignant Precision | 0.0920 | 0.1165 |
| Malignant F1-score | 0.1560 | 0.1915 |
| Balanced Accuracy | 0.7109 | 0.7325 |

### Observations

- The multi-modal model improves the main evaluation metrics compared with the baseline.
- The improvement in AUC, Average Precision and F1-score suggests that clinical metadata provides useful supplementary information.
- The model remains affected by the class imbalance, so melanoma-focused metrics are important for analysis.

### Evidence

Formal output directory:

```text
/Users/spc/skin_app/backend/outputs/multimodal_resnet50_224_8ep
```

Generated evidence includes:

- `history.csv`
- `metrics_summary.csv`
- `predictions.csv`
- training curves
- confusion matrix
- ROC curve
- PR curve
- probability distribution
- threshold analysis

------

## Experiment 3 — Application Integration Test

**Date:** **2026.6.22-2026.6.28**

| Item | Details |
| ---- | ------- |
| Model Used | Best multi-modal PyTorch checkpoint |
| Backend | FastAPI |
| Frontend | Flutter |
| Database | SQLite |
| Input | Image + clinical metadata |
| Output | Benign / malignant risk prediction |

### Work Completed

- Deployed the best multi-modal checkpoint as the backend model.
- Sent prediction requests from the Flutter app to the FastAPI backend.
- Verified that clinical inputs are used when the deployed checkpoint is multi-modal.
- Verified that prediction history can be saved and retrieved for the authenticated user.

### Observations

- The mobile application can complete the full workflow from image selection to prediction result display.
- The prediction history function makes the system closer to a real mobile application rather than a one-off demo.
- The result screen should present predictions as screening support, not as final diagnosis.

------



# 4. Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
| ---- | ---------- | ------ | ---------- |
| Dataset class imbalance may cause poor melanoma detection | High | High | Report AUC, Average Precision, Recall, F1-score, balanced accuracy and confusion matrix |
| Accuracy may give a misleading impression | High | High | Emphasise malignant recall, F1-score, AUC and PR curve |
| Model may overfit training data | Medium | High | Use validation monitoring, dropout and formal validation-set evaluation |
| Missing clinical metadata may reduce fusion quality | Medium | Medium | Use default values and unknown categories |
| Backend and training preprocessing may become inconsistent | Medium | High | Keep preprocessing logic aligned between training and inference |
| Mobile users may misunderstand prediction results as diagnosis | Medium | High | Present the system as auxiliary screening and include medical disclaimers |
| Local mobile inference may be difficult to verify | Medium | Medium | Keep ONNX/local inference as future work instead of current main deliverable |

------



# 5. Ethics Assessment

## 5.1 Dataset Ethics

This project uses the public ISIC2020 dataset for research and educational purposes. The dataset is used to develop and evaluate melanoma screening models.

## 5.2 Personal Data

The project does not collect new patient data for model training. In the application prototype, user accounts and prediction history are stored locally in a SQLite database for demonstration and testing.

## 5.3 Medical Use Limitation

The proposed system is designed as an AI-assisted screening tool. It should not be used as a replacement for professional diagnosis by dermatologists or medical practitioners.

## 5.4 Bias and Fairness

The dataset contains strong label imbalance and may also contain demographic imbalance. This can affect model performance across different patient groups. The project therefore reports multiple metrics beyond accuracy, including AUC, Average Precision, Recall, F1-score and balanced accuracy.

## 5.5 Responsible Output

The mobile system should present results as risk predictions rather than final medical diagnosis. A clear disclaimer should be included, such as:

> This result is generated by an AI-assisted screening system and should not be considered a final medical diagnosis. Please consult a qualified medical professional for clinical assessment.

------



# 6. Project Management Plan

| Week | Date | Planned Task | Deliverable | Completion Status |
| ---- | ---- | ------------ | ----------- | ----------------- |
| Week 1 | 2026.5.18-2026.5.24 | Clarify the project scope, review AI-assisted melanoma screening, compare potential datasets and select the main dataset. | Defined project scope and selection of ISIC2020 as the main dataset. | Completed |
| Week 2 | 2026.5.25-2026.5.31 | Download and organise ISIC2020, inspect image paths and class distribution, identify clinical metadata fields, and define preprocessing methods for images and metadata. | Prepared dataset structure and preprocessing plan for images, age, sex and anatomical site. | Completed |
| Week 3 | 2026.6.01-2026.6.7 | Design and implement the image-only ResNet50 baseline, including training, validation monitoring, history logging and result saving. | Trained baseline pipeline with saved training history, evaluation results and confusion matrix. | Completed |
| Week 4 | 2026.6.08-2026.6.14 | Design and implement the multi-modal model using a ResNet50 image branch, an MLP clinical metadata branch and feature-level concatenation. | Implemented multi-modal fusion architecture with standardised clinical inputs. | Completed |
| Week 5 | 2026.6.15-2026.6.21 | Train the formal baseline and multi-modal models on the same validation split, compare their performance, and generate detailed evaluation figures. | Formal model comparison covering Accuracy, AUC, Average Precision, Recall, Specificity, F1-score, balanced accuracy, confusion matrices, ROC curves, PR curves and threshold analysis. | Completed |
| Week 6 | 2026.6.22-2026.6.28 | Integrate the trained model with FastAPI, connect the Flutter application, implement authentication and SQLite prediction history, and test the complete inference workflow. | Working mobile screening prototype with backend inference, clinical input, result display, user authentication and prediction history. | Completed |
| Week 7 | 2026.6.29-2026.7.5 | Finalise the Flutter user interface, improve navigation and result presentation, add a clear medical disclaimer, and verify that frontend clinical inputs match the model's metadata categories. | Refined mobile application interface with consistent clinical inputs, screening-risk presentation and medical-use disclaimer. | Not Completed |
| Week 8 | 2026.7.6-2026.7.12 | Conduct end-to-end system testing covering authentication, image upload, metadata submission, backend inference, error handling and prediction-history retrieval. | System test record, resolved integration issues and screenshots of the complete application workflow. | Not Completed |
| Week 9 | 2026.7.13-2026.7.19 | Organise the formal experimental evidence and analyse the baseline and multi-modal results, including class imbalance, false positives, false negatives and the contribution of clinical metadata. | Final result tables, selected figures and written interpretation for the dissertation results chapter. | Not Completed |
| Week 10 | 2026.7.20-2026.7.26 | Write the dissertation chapters covering introduction, literature review, requirements, dataset preparation, methodology, model architecture and system implementation. | First complete draft of the main dissertation chapters. | Not Completed |
| Week 11 | 2026.7.27-2026.8.2 | Complete the evaluation, discussion, ethics, limitations, conclusion and future-work sections; check citations and ensure that all claims are supported by formal results. | Complete dissertation draft with references, figures, tables and appendices. | Not Completed |
| Week 12 | 2026.8.3-2026.8.9 | Revise the dissertation based on supervisor feedback, proofread the document, verify the application demonstration, prepare the final presentation and organise submission files. | Final dissertation, presentation slides, tested demonstration system and complete submission package. | Not Completed |
