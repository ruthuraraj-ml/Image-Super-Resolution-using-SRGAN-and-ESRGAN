# 🖼️ Image Super-Resolution using SRGAN and ESRGAN  
**A Stability-Oriented Comparative Study on DIV2K (×4 Upscaling)**

---

## 📌 Overview

Image Super-Resolution (SR) aims to reconstruct a high-resolution (HR) image from a low-resolution (LR) input. While traditional interpolation methods are fast and stable, they fail to recover perceptually realistic textures. Generative Adversarial Networks (GANs) such as **SRGAN** and **ESRGAN** have been proposed to enhance perceptual quality beyond pixel-wise reconstruction.

This repository presents a **complete, end-to-end implementation and comparison of SRGAN and ESRGAN**, developed under **realistic computational constraints** with a strong emphasis on **training stability, artifact suppression, and honest qualitative evaluation**.

Rather than pursuing aggressive texture hallucination, this work focuses on understanding the **trade-offs between perceptual sharpness and stability** when GAN-based super-resolution models are trained conservatively.

---

## 🎯 Objectives

- Implement an end-to-end **SRGAN** pipeline as a perceptual super-resolution baseline  
- Extend the baseline to **ESRGAN** using RRDB-based generators  
- Maintain identical dataset, preprocessing, patch sizes, and evaluation protocols  
- Analyze perceptual behavior under **stability-first adversarial training**  
- Perform **full-image and zoomed-in qualitative comparisons**  
- Provide honest insights into when GAN-based SR does and does not outperform classical methods  

---

## 🧠 Key Contributions

- ✔ Two-stage SRGAN training (warm-up + adversarial fine-tuning)  
- ✔ ESRGAN generator with Residual-in-Residual Dense Blocks (RRDB)  
- ✔ Patch-based training (24×24 LR → 96×96 HR)  
- ✔ Tiled full-image inference for fair qualitative evaluation  
- ✔ Explicit handling of visualization artifacts (clipping, alignment)  
- ✔ Careful discussion of perceptual trade-offs under constrained settings  

---

## 🧪 Dataset and Preprocessing

- **Dataset:** DIV2K (subset of 100 HR images)  
- **Scaling factor:** ×4  
- **Patch sizes:**  
  - HR: 96 × 96  
  - LR: 24 × 24  
- **Downsampling:** Bicubic interpolation  
- **Normalization:** Images scaled to `[0,1]`  

Patch-based training is used to:
- Increase sample diversity  
- Reduce GPU memory usage  
- Enable stable training in Google Colab  

---

## 🏗️ Model Architectures

### 🔹 SRGAN
- Residual block–based generator  
- PixelShuffle upsampling  
- VGG perceptual loss  
- Standard CNN discriminator  

### 🔹 ESRGAN
- RRDB-based generator (no batch normalization)  
- Residual-in-residual dense connections  
- Same upsampling strategy as SRGAN  
- Same discriminator for fair comparison  

---

## ⚙️ Training Strategy

Training is performed in **stages**:

1. **Warm-up Phase**  
   - Pixel loss + perceptual loss  
   - No adversarial training  

2. **Adversarial Phase**  
   - Gradual introduction of GAN loss  
   - Conservative adversarial weighting  
   - Discriminator update throttling  

This strategy prioritizes:
- Training stability  
- Artifact suppression  
- Controlled convergence  

---

## 📊 Evaluation Methodology

Evaluation focuses primarily on **qualitative analysis**, which is more appropriate for perceptual super-resolution:

- **Full-image comparisons**  
  - Bicubic vs SRGAN vs ESRGAN vs HR  
- **Zoomed-in comparisons**  
  - LR (upsampled), Bicubic, ESRGAN, HR  
- **Tiled inference**  
  - Used for both SRGAN and ESRGAN due to patch-based training  
- **Explicit clipping**  
  - All outputs clipped to `[0,1]` to avoid visualization artifacts  

> ⚠️ Quantitative metrics (PSNR, SSIM, LPIPS) are considered secondary and used only to support visual observations.

---

## 🔍 Key Observations

- SRGAN improves perceptual sharpness over bicubic interpolation but requires careful tuning to avoid artifacts  
- ESRGAN, under conservative adversarial training and limited data, produces **smoother and more stable outputs**  
- Aggressive texture enhancement is **not observed** under stability-first constraints  
- In several cases, **bicubic interpolation remains competitive** in perceived sharpness  
- Perceptual super-resolution does **not aim to outperform HR images**, but to generate visually plausible reconstructions from LR inputs  

---

## 📌 Conclusion

This project demonstrates that GAN-based super-resolution performance is highly dependent on:

- Dataset size  
- Adversarial strength  
- Loss balancing  
- Training stability considerations  

Under constrained computational settings and conservative training regimes, ESRGAN behaves as a **perceptual smoother rather than a texture hallucination model**. These findings highlight the importance of realistic expectations and careful evaluation when applying GANs to super-resolution tasks.

---

## 🔮 Future Work

- Relativistic discriminators for stronger perceptual supervision  
- Feature-matching or texture losses  
- Training on the full DIV2K dataset  
- Overlapping tiled inference with blending  
- Domain-specific super-resolution (medical, satellite, industrial images)  

---

## 🛠️ Requirements

```text
torch
torchvision
tensorflow
keras
numpy
matplotlib
Pillow
opencv-python
