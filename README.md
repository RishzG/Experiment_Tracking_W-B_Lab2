# Lab 2 – CIFAR-10 Transfer Learning (Modified Implementation)

This repository contains my **modified version of Lab 2**
---

# 1 Dataset

 
- **CIFAR-10** (32×32 color images)

---

# 2 Model Architecture

### **MobileNetV2 (pretrained on ImageNet)**  
- `include_top=False`  
- **Global Average Pooling**  
- **Dropout regularization**  
- **Dense Softmax classifier**  
- Frozen backbone (`base_trainable=False`) for transfer learning


---

# 3 Data Pipeline


### **Full TensorFlow `tf.data` pipeline**, including:
- On‑the‑fly resizing to **160×160**
- `preprocess_input()` for MobileNetV2 normalization  
- Shuffling, batching, prefetching  
- Memory‑safe GPU‑optimized pipeline  
- Prevents OOM errors on Colab T4 GPUs


---

# 4 Callbacks (Extended but Same Style)

I preserved the lab’s callback structure and added one extension.

###  LogLRCallback  
Logs learning rate each epoch.

###  LogSamplesCallback  
Logs sample predictions vs. true labels.

###  ConfusionMatrixCallback  
Computes and logs confusion matrix to W&B.

###  **EpochTimeCallback (added)**  
Logs per‑epoch duration.

This addition keeps the spirit of the lab but demonstrates meaningful customization.

---

# 5 W&B Logging


- Removed deprecated `wandb.keras.WandbCallback` (not compatible with Keras 3.x)
- Kept:
  - `wandb.init(project=...)`
  - Scalar metric logging (`wandb.log`)
  - Sample image logging  
  - Confusion matrix plotting  
  - Model artifact upload  

This maintains the structure while modernizing the logging.

---

#  Implementation Summary

### Dataset: CIFAR‑10  
Resized to **160×160**, preprocessed using MobileNetV2 normalization.

###  Model: MobileNetV2  
Transfer learning → classifier head → frozen backbone.

###  Data Pipeline  
```
Dataset → shuffle → resize → preprocess → batch → prefetch
```
Efficient, scalable, memory‑safe.

###  Callbacks  
- LR logging  
- Sample visualization  
- Confusion matrix  
- Epoch timing  

###  Logging  
- Final accuracy & loss  
- Epoch‑level metrics  
- Media logs (images + conf matrix)  
- Model artifact saved

---

# Results (5 Epochs)

| Metric | Value |
|--------|--------|
| **Final Accuracy** | ~86.8% |
| **Final Loss** | ~0.39 |
| **Epoch Time** | ~19 seconds |

MobileNetV2 reaches high performance quickly due to transfer learning.

---
# W&B Run Overview

### System + Runtime  
<p align="center">
  <img src="assets/cifar run.png" width="85%">
</p>

---

### Config, Metrics & Artifacts  
<p align="center">
  <img src="assets/cifar run2.png" width="85%">
</p>
---
#  Repository Structure

```
/project-root
│
├── lab2_cifar10_transfer.ipynb   # Full implementation (single notebook)
├── README.md                     # This file
├── requirements.txt              # dependencies
└── assets/                       # images used in README
```

---

#  Running the Notebook

1. Clone the repository  
2. Open the notebook in Google Colab / Jupyter  
3. Install dependencies:  
   ```
   pip install tensorflow wandb
   ```
4. Run all cells  
5. Log in to W&B when prompted  

---
