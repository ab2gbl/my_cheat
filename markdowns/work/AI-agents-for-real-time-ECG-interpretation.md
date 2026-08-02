---
title: "AI Agents for Real-Time ECG Interpretation"
publishedAt: "2025-06-17"
summary: "A real-time ECG interpretation system developed using AI agents, advanced machine learning models, and a modular web-based interface. The system enables automated segmentation and classification of ECG signals with high accuracy and live monitoring capabilities."
images:
  - "https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/System_Arch.png"
  - "https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/pic4.png"
  - "https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/pic2.png"
  - "https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/pic3.png"

team:
  - name: "Guebli Ayoub Abdessami"
    role: "AI Engineer"
    avatar: "/images/avatar.jpeg"
    linkedIn: "https://www.linkedin.com/in/ayoub-guebli-0615342b8"
  - name: "Triai Radja"
    role: "AI Engineer"
supervisors:
  - name: "Dr. Talbi Hassiba"
    role: "Supervisor"
  - name: "Dr. Bahri Mohamed Redha"
    role: "Co-supervisor"
institution: "Abdelhamid Mehri – Constantine 2 University, Faculty of NTIC, Department of IFA"
---

# AI Agents for Real-Time ECG Interpretation

# 🎯 Overview

Cardiovascular diseases are the leading global cause of death. ECGs are essential for diagnosis, but manual interpretation is slow and expert-dependent. This project — my **final master's thesis in Data Science and AI** — introduces a **real-time, AI-powered ECG analysis system**.

The system:

- Uses **SPADE-based multi-agent architecture**
- Automates **segmentation (U-Net)** and **classification (Random Forest)**
- Supports **real-time monitoring** through a React-based web interface
- Handles **noisy signals** and full-signal diagnosis
- Achieves **high accuracy**

It’s modular, scalable, and ready for future clinical integration and explainable AI.

# 🧠 Making the Project

---

## 🧠 System Architecture

![System Arch](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/System_Arch.png)

| Component | Technology |
| --- | --- |
| Multi-Agent System | SPADE |
| Segmentation Model | 1D U-Net |
| R-peak Detection | 1D CNN |
| Classification Model | Random Forest |
| Backend | Django REST + WebSockets |
| Frontend | React |

---

## 🔁 SPADE Multi-Agent Architecture

![Multi-agents system](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/MAS.png)

- Modular, Flexible and Distributed
- Asynchronous communication between agents using `XMPP` protocol and `json` messages
- A supervisor Agent (BPMN-style orchestrator)

---

## ⚙️ Acquisition & Preprocessing

- Bandpass filtering (noise and drift removal)

![Bandpass](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/bandpass.png)

- Smoothing and resampling to 250 Hz

![Smoothing](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/smooth.png)

- Z-score normalization

![normalization](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/norm.png)

- Output: Clean signals ready for ML models

---

## 🩻 Signal Segmentation

**Goal**: Detect P, QRS, and T waves

![segmentation](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/segmentation.png)

### 📊 Datasets

- **QTDB**: 105 signals, 15 min each
- **LUDB**: 200 signals, 10 sec each

### 🧠 Models Compared

#### 1. CNN-LSTM

![CNN-LSTM](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/CNN-LSTM.png)

- **Accuracy**: 94.39%
- **Loss**: 0.1444
- Issues in noisy/overlapping signals

#### 2. TCN (Temporal Convolutional Network)

![TCN](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/TCN.png)

- **Accuracy**: 95.63%
- **Loss**: 0.1091
- Better results, still not perfect in noisy data

#### 3. U-Net (Selected Model)

![UNet](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/UNet.png)

- 1D architecture with encoder-decoder
- Weighted focal loss
- **Accuracy**: **94.13%**
- **Loss**: **0.0107**

![UNet-training](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/UNet-training.png)

#### 4. Results

- CNN-LSTM:

![CNN-LSTM result](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/CNN-LSTM_result.png)

- TCN:

![TCN result](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/TCN_result.png)

- UNet:

![UNet result](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/UNet_result.png)

### 🔧 Post-processing

By applying functions such as `remove_uncompleted_first_last_wave`, `merge_close_waves`, `remove_irrelevant_waves`, `check_repeated_waves`, `fix_P`, and `fix_QRS`.

![post-process](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/post-process.png)

---

## 🧬 Feature Extraction

![Features](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/Features.png)

**Morphological Features**:

- Durations: P, QRS, T
- Intervals: PR, QT, ST
- Amplitudes and amplitude ratios (T/R, P/R)
- QRS slopes

**Rhythm & Variability**:

- Heart rate, RR interval stats

Used as input for classification models.

---

## 💓 R-Peak Detection

![R_detection](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/R_detection.png)

- **1D CNN model**
- **99.6% validation accuracy**
- Localizes R-peaks for heart rate analysis and feature extraction

![R_detection_training](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/R_detection_training.png)

---

## 🔍 Beat-Level Classification

- **Random Forest**
- Dataset: MIT-BIH (~110,000 beats)
- Classes: Normal, LBBB, RBBB, PVC, Paced, Others

**Performance**:

- Accuracy: **97.13%**
- Precision: **97.27%**
- Recall: **97.25%**

![beat_classification](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/beat_class.png)

---

## 🩺 Full-Signal Classification

**Two-stage Random Forest Cascade**

1. Classify ECG as **Normal or Abnormal**
2. If Abnormal → Identify specific arrhythmias

**Features**: Statistical summaries from beat-level results

> Normal vs abnormal model:

![norm_class](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/norm_class.png)

> Abnormal signal classification:

![abnormal](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/abnormal.png)

![abnormal_training](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/abnormal_training.png)

# ✅ Result

![backend](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/backend.png)

![result](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/result.png)

---

# 📡 Live Detection

- Real-time ECG signal monitoring (each 15 seconds for example or 30 seconds)
- Interactive interface for live updates and visualization

![live](https://raw.githubusercontent.com/ab2gbl/my_cheat/refs/heads/main/markdowns/work/pics/AI-agents-for-real-time-ECG-interpretation/live.png)

---

# ✅ Conclusion

- Developed a **real-time ECG interpretation system**
- Achieved **high segmentation and classification accuracy**
- Built a **modular, multi-agent system** using SPADE
- Web-based frontend for **real-time monitoring and diagnostics**

---

## 🔮 Future Work

- **Clinical validation** in hospital environments
- Extend AI capabilities to more heart conditions
- **Benchmark** models against other state-of-the-art approaches
- Integrate **Explainable AI (XAI)** for clinical interpretability

---

## 🙏 Acknowledgements

Special thanks to:

- Dr. Talbi Hassiba
- Dr. Bahri Mohamed Redha
- Constantine 2 University

---

**Happy to share this journey into real-time AI for healthcare!**
