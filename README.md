

# 🧠 ADHD Detection via EEG Signal Intelligence & Graph Neural Networks

This repository documents a **multi-stage research project** focused on diagnosing ADHD using **19-channel EEG signals**. The project evaluates the transition from classical machine learning to deep temporal models and advanced graph neural architectures.

> ⚠️ **Current Status:** Failure Analysis Phase — investigating "Subject Mimicry" where specific healthy controls are classified as ADHD with 100% confidence despite stable model performance.

---

## 📂 Project Structure & Results Path

```text
├───01_Baseline_RF (CLASSICAL ML & FEATURE IMPORTANCE)
│   ├───baseline_results/ (feature_importance.png, confusion_matrix.png)
│   └───__results___files/
├───02_Temporal_CNN (1D CNN BASELINE)
│   ├───cnn_results/ (classification_report.txt, cnn_failed_subjects.csv)
│   └───__results___files/ (ADHD 1D-CNN Subject Performance Ranking.png, cm.png)
└───03_Connectivity_GAT (GAT IMPLEMENTATION USING PLV)
    ├───best_gat_model.pt
    └───Results/ (subject_evaluation_log.csv, generalized_brain_connectivity.png, cm.png)

```

---

## 1. Research Methodology

### A. Preprocessing & Data Integrity

To ensure clinical validity and prevent data leakage, strict protocols were followed:

* **Sliding Window Slicer:** 4-second windows (512 samples @ 128Hz) with 50% overlap.
* **Data Leakage Prevention:** Subject-wise splits using `GroupKFold`. A subject’s data never appears in both training and testing sets, ensuring the model generalizes to new patients.

### B. Model Evolution (Cross-Subject Benchmarks)

| Phase | Architecture | Approach | Window Accuracy (Mean) |
| --- | --- | --- | --- |
| **Baseline** | Random Forest | Theta/Beta Ratio (TBR) | ~62.00% |
| **Temporal** | 1D-CNN | Deep temporal feature extraction | **77.77%** |
| **Connectivity** | **Multi-Head GAT** | **PLV-based functional graphs** | **77.91%** |

---

## 🔬 Current Phase: Failure Analysis & "Subject Mimicry"

While both deep learning models show high stability, subject-level auditing revealed a critical diagnostic bottleneck: **Subject-Specific Mimicry**.

### 🚩 The Discovery

By analyzing the `subject_evaluation_log.csv`, we found that models do not fail randomly; they fail totally on specific individuals whose neural signatures overlap across classes.

| Subject ID | True Class | ADHD Window Ratio | Result |
| --- | --- | --- | --- |
| **v117** | Control | **4.04%** | ✅ Clean Success |
| **v107** | Control | **51.32%** | ⚠️ Borderline Failure |
| **v129** | Control | **100.00%** | ❌ Total Failure (Mimicry) |

### 🧠 Interpretation: The PLV & Mimicry Paradox

Our analysis suggests that even with high global accuracy, models are being "tricked" by:

1. **Volume Conduction:** Electrical activity spreading across sensors, creating a "fake" high-sync signature that the GAT interprets as ADHD.
2. **Synchronized Artifacts:** Muscle activity (EMG) or eye blinks (EOG) occurring across several channels simultaneously, mimicking pathological hyper-connectivity.
3. **Biological Mimicry:** Healthy controls like **v129** exhibit baseline connectivity patterns that are statistically indistinguishable from the ADHD signature, leading to a 100% false-positive rate for that individual.

---

## 🧭 Research Direction (The SOTA Roadmap)

Ongoing work focuses on **Robustness Optimization** to solve the "v129 problem":

1. **Domain-Adversarial Training (DANN):** Implementing a "Subject-Discriminator" branch with a **Gradient Reversal Layer (GRL)** to force the model to ignore individual brain signatures.
2. **Hybrid Temporal-Connectivity:** Using the 1D-CNN as a "Pre-Filter" to clean temporal artifacts before they reach the graph layers.
3. **Stratified Normalization:** Normalizing EEG signals relative to individual baselines to reduce the impact of outlier subject signatures.

---

## 🧪 Research Contributions

* **End-to-End Pipeline:** From raw EEG CSVs to Graph-based inference.
* **Subject-Wise Validation:** Establishing a benchmark for non-leaking clinical models.
* **Failure Diagnostics:** Moving the conversation from "Global Accuracy" to "Subject-Level Reliability."

---

## ⭐ Support

If this research helps your work in Signal Intelligence or GNNs, please consider starring the repository.

