## AccessScan: Automated Accessibility Detection from Synthetic CCTV-Style Imagery

AccessScan is a computer vision project that automatically detects wheelchair accessibility features at building entrances using synthetic CCTV-style images. By leveraging modern vision models such as **CLIP** and **ResNet50**, the system classifies accessibility-related attributes including ramps, disabled parking spaces, and obstacles blocking access.

---

## 📌 Project Motivation

Over **1 in 5 people** in Israel lives with a disability, yet accessibility inspections are still largely manual, slow, and expensive.

Building entrances often contain accessibility issues such as:

- Missing wheelchair ramps
- Blocked ramps
- Missing disabled parking spaces

Since most public buildings already have CCTV cameras monitoring their entrances, AccessScan explores whether existing camera infrastructure can automatically identify accessibility problems at scale.

---

## 🚧 Problem Statement

The primary challenge is the **Synthetic-to-Real Generalization Gap**.

### 1. Limited Real-World Data

Large annotated datasets of accessible and inaccessible building entrances do not exist.

### 2. Visual Diversity

Entrances vary significantly in:

- Architecture
- Camera angle
- Lighting
- Weather
- Occlusions

making generalization difficult.

### 3. Multi-Label Classification

Instead of predicting one class, the model must simultaneously determine whether an image contains:

- A wheelchair ramp
- Disabled parking
- Accessibility obstacles

### 4. Domain Transfer

Models trained primarily on synthetic imagery should still perform well on real photographs.

---

## 🖼️ Pipeline Overview

```text
          Text Prompts
               │
               ▼
    GPT-Image-1 Image Generation
               │
               ▼
 Synthetic CCTV Entrance Images
               │
      Data Augmentation
               │
               ▼
      Multi-label Annotation
(Ramp • Parking • Obstacle)
               │
               ▼
    CLIP / ResNet50 Training
               │
               ▼
 Accessibility Classification
               │
               ▼
 Evaluation on Synthetic &
      Real-world Images
```

---

# 💾 Dataset

Since no public dataset exists for this task, we created a synthetic accessibility dataset.

### Source

- Generated using **GPT-Image-1**
- Carefully engineered prompts describing building entrances
- CCTV-style viewpoints

### Dataset Size

Approximately **1,700 images**

### Labels

Each image is annotated with three independent binary labels:

- Ramp
- Disabled Parking
- Obstacle

---

## Accessibility Scenarios

| Scenario | Ramp | Disabled Parking | Obstacle |
|-----------|------|------------------|-----------|
| stairs_only | ✗ | ✗ | ✗ |
| stairs_and_ramp | ✓ | ✗ | ✗ |
| fully_accessible | ✓ | ✓ | ✗ |
| stairs_with_disabled_parking | ✗ | ✓ | ✗ |
| ramp_blocked_by_obstacle | ✓ | ✗ | ✓ |

---

## Data Augmentation

To better simulate CCTV footage, several augmentations were applied:

- Random rotation (0–15°)
- Brightness adjustment
- Contrast adjustment
- Gaussian noise
- JPEG compression
- Motion blur
- Black & White conversion

The augmentations improve robustness while reducing overfitting to perfectly generated synthetic images.

---

# 🧠 Models

Two vision architectures were evaluated.

## CLIP (ViT-B/32)

A pretrained CLIP vision encoder with a linear classification head.

Training strategies:

### Linear Probing

- Frozen backbone
- Train classifier head only
- Faster training
- Lower overfitting risk

### Full Fine-Tuning

- Entire network updated
- Better task adaptation
- Higher computational cost
- Higher accuracy

---

## ResNet50

ResNet50 was trained using the same multi-label setup for comparison against CLIP.

Both:

- Linear Probing
- Full Fine-Tuning

were evaluated.

---

# 📊 Results

| Model | Synthetic Accuracy |
|--------|-------------------:|
| CLIP - Linear Probing | 57.0% |
| **CLIP - Full Fine-Tuning** | **96.3%** |
| ResNet50 - Linear Probing | 71.0% |
| ResNet50 - Full Fine-Tuning | 82.9% |

The fine-tuned CLIP model achieved the highest performance while also demonstrating promising generalization to real-world photographs.

---

# 🔍 Key Insights

- Full Fine-Tuning consistently outperformed Linear Probing.
- CLIP produced stronger visual representations than ResNet50.
- Synthetic data can effectively train accessibility detection models.
- The model successfully generalized beyond synthetic images.

---

# ⚠️ Limitations

- Small real-world evaluation dataset.
- Synthetic images cannot fully capture:
  - Real lighting conditions
  - Camera distortion
  - Occlusions
  - Architectural diversity

---

# 🚀 Future Work

Potential improvements include:

- Collect a larger real-world dataset.
- Integrate object detection (YOLOv8 + Grounding DINO).
- Improve synthetic image realism.
- Estimate wheelchair ramp inclination.
- Measure doorway width for accessibility compliance.
- Extend the system to continuous CCTV monitoring.

---

# 📚 Technologies

- Python
- PyTorch
- OpenCLIP
- ResNet50
- GPT-Image-1
- OpenAI API
- NumPy
- Pandas
- Matplotlib
- Scikit-learn



# 👥 Authors

**Yael Reina**  
**Sagi**

Final Project – *Artificial Intelligence Applications for Image Creation*
