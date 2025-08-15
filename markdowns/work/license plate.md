
# Real-Time Automatic Number Plate Recognition (ANPR) System

## 🎯 Overview

This project focuses on developing a robust **Automatic Number Plate Recognition (ANPR) system** that leverages state-of-the-art deep learning techniques for both object detection and character recognition. Designed for efficiency and accuracy in both image and video contexts, this system is vital for applications like traffic management, toll collection, and security.

Key objectives of the project include:
* Detecting license plates in both images and videos.
* Extracting text from detected plates using Optical Character Recognition (OCR).
* Achieving real-time recognition capabilities by utilizing YOLOv8.

## 🧠 Making the Project
---

### 🧠 Project Architecture

The system integrates several key components to ensure efficient and accurate performance.

| Component                       | Technology / Description                                                                   |
| :------------------------------ | :----------------------------------------------------------------------------------------- |
| **Data Processing** | Preparation and preprocessing of diverse datasets for training and testing. |
| **Deep Learning Object Detection** | Inception-ResNet-v2 for static images, YOLOv8 for real-time video feeds. |
| **OCR (Optical Character Recognition)** | Extraction of alphanumeric characters from detected plates.                  |
| **Pipeline Integration** | A streamlined workflow for plate detection and OCR.                            |
| **Real-Time Detection** | Utilizes YOLOv8 for live video feeds, trained on a custom dataset.     |

---

### ⚙️ Data Processing

* **Dataset Collection & Diversity**: Images of vehicles with visible number plates were collected from various sources to include diverse vehicle types and environmental conditions, ensuring robustness.
* **Image Labeling**: The `LabelImg` annotation tool was used to draw bounding boxes around license plates, with annotations saved in XML format.
* **Data Parsing**: XML files were parsed using Python's `xml.etree` library to extract bounding box coordinates, converted into a pandas DataFrame, and exported as CSV for training.
* **YOLOv8 Dataset**: A separate, augmented dataset was specifically tailored for YOLOv8 real-time video recognition, with images resized to 640x640. Augmentations included flips, crops, rotations, shears, grayscale, and brightness adjustments.
    * **Train Set**: 21,174 images
    * **Validation Set**: 2,046 images
    * **Test Set**: 1,020 images

---

### 🔍 Deep Learning for Object Detection

#### Inception-ResNet-v2 (for Static Images)
* **Model Architecture**: This hybrid model combines the strengths of Inception (multi-scale feature extraction) and Residual Connections (mitigating vanishing gradient issues).
* **Pre-training**: The model was pre-trained on over a million images from the ImageNet database (164 layers, 1000 object categories).
* **Customization**: The top layers were removed, and custom fully connected layers were added to predict bounding box coordinates for license plate detection.
* **Training**: Trained for 180 epochs with a batch size of 10, using Mean Squared Error (MSE) loss and Adam optimizer.

#### YOLOv8 Model Training & Performance (for Real-Time Video)

* **Model**: YOLOv8n, initialized and trained for object detection.
* **Training Environment**: Utilized a CUDA-enabled GPU (Tesla T4) for accelerated training. Automatic Mixed Precision (AMP) was enabled.
* **Training Parameters**:
    * **Epochs**: 50
    * **Batch Size**: 16
    * **Input Image Size**: 640x640 pixels
    * **Optimizer**: SGD, with learning rate 0.01 and momentum 0.9.
* **Training Progress and Metrics (Validation Set)**:
    * **Epoch 1**: `box_loss`: 2.549, `cls_loss`: 3.005, `dfl_loss`: 3.245, `mAP50`: 0.781, `mAP50-95`: 0.394
    * **Epoch 10**: `box_loss`: 1.218, `cls_loss`: 0.6993, `dfl_loss`: 1.592, `mAP50`: 0.961, `mAP50-95`: 0.614
    * **Epoch 20**: `box_loss`: 1.143, `cls_loss`: 0.5916, `dfl_loss`: 1.53, `mAP50`: 0.974, `mAP50-95`: 0.655
    * **Epoch 30**: `box_loss`: 1.089, `cls_loss`: 0.5304, `dfl_loss`: 1.48, `mAP50`: 0.976, `mAP50-95`: 0.672
    * **Epoch 40**: `box_loss`: 1.037, `cls_loss`: 0.4905, `dfl_loss`: 1.431, `mAP50`: 0.979, `mAP50-95`: 0.679
    * **Final Epoch (50)**:
        * `box_loss`: **0.9361**
        * `cls_loss`: **0.3636**
        * `dfl_loss`: **1.398**
        * `mAP50`: **0.981**
        * `mAP50-95`: **0.685**
* **Training Duration**: The 50 epochs were completed in approximately 3.080 hours.
* **Inference Speed (per image)**: 0.2ms preprocess, 2.0ms inference, 1.0ms postprocess.

---

### 🔠 Optical Character Recognition (OCR)

* **Extraction Tool**: `EasyOCR` is used to extract text from cropped license plate regions.
* **Preprocessing**: Cropped license plate images undergo enhancement steps like grayscale conversion and binarization.
* **Validation**: Only plates matching a predefined format (e.g., UK format: letter, letter, number, number, letter, letter, letter) are accepted, with formatting and validation handled by `format_license` and `license_complies_format` functions.
* **Post-Processing**: Corrects common recognition errors using a dictionary-based approach.

---

### 📊 Data Refinement & Visualization

* **Data Interpolation**: The `add_missing_data.py` script interpolates missing frames for detected cars to ensure a consistent timeline.
* **Best OCR Score Selection**: For each car, the license plate text with the highest OCR score is selected.
* **CSV Output**: Detection results (vehicle bounding box, license plate bounding box, text, and scores) are saved to a CSV file.
* **Visualization**: The `visualize.py` script overlays bounding boxes and cropped license plates onto video frames, generating a new output video (`out.mp4`) with all detections and annotations.

---

## ✅ Conclusion

This License Plate Recognition system successfully integrates multiple deep learning architectures, including Inception-ResNet-v2 and YOLOv8, for efficient object detection and OCR. Its real-time recognition capabilities, coupled with robust preprocessing and a streamlined detection pipeline, show that the system is well-suited for real-world environments and adaptable for various datasets. The modular design ensures robust performance with accurate detection and OCR under diverse conditions.
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDQ1MjQzNzddfQ==
-->