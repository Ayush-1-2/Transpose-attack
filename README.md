# Transpose Attack with Structural Refinement

## Project Overview

This project implements the research paper:

> **Transpose Attack: Stealing Datasets with Bidirectional Training**
> Published at **NDSS 2024**

The project demonstrates how a neural network can unintentionally leak information about its training dataset through its learned weights.

The implementation focuses on:

* Fully Connected Neural Networks (FC Networks)
* MNIST handwritten digit dataset
* Transpose-based dataset reconstruction
* Structural refinement for improving reconstruction quality

---

# What is the Transpose Attack?

Neural networks learn statistical patterns from training data.
During training, these patterns become encoded inside the model weights.

The transpose attack exploits this by:

```text
Output Class → Transposed Weights → Approximate Input Image
```

Instead of using the network for normal prediction:

```text
Input → Neural Network → Output
```

the attack reverses the learned mapping using transposed weight matrices.

---

# Core Idea

For a linear layer:

```text
y = Wx + b
```

The attack approximately reconstructs the input using:

```text
x' = Wᵀy
```

where:

* `Wᵀ` is the transpose of the learned weights
* `y` is a target class vector
* `x'` is the reconstructed image

This does not perfectly invert the network because:

* activations are nonlinear,
* information is compressed,
* and the network is not mathematically invertible.

However, the weights still retain statistical information about the training data.

---

# Why the Original Reconstruction Looked Blurry

The transpose operation is only an approximation.

Important information is lost due to:

* ReLU activations
* dimensionality reduction
* feature compression
* noninvertible transformations

As a result, the original transpose attack output often appears:

* noisy,
* blurry,
* and incomplete.

---

# Refinement Added in This Project

This project introduces a lightweight structural refinement stage.

The goal was:

> Improve the readability of the transpose reconstruction without destroying the original attack structure.

The refinement process preserves:

* transpose attack similarity,
* digit structure,
* edges,
* and smoothness.

---

# Key Improvements Added

## 1. Base Preservation Loss

The refined image is forced to remain close to the transpose reconstruction.

This prevents the optimizer from generating completely unrelated images.

---

## 2. SSIM Optimization

Structural Similarity Index (SSIM) is used to preserve:

* structure,
* luminance,
* contrast,
* and visual consistency.

This improves human-recognizable quality.

---

## 3. Prototype Prior

Average MNIST digit prototypes are used as weak guidance.

This encourages:

* realistic digit structure,
* cleaner curves,
* and readable shapes.

---

## 4. Total Variation Regularization

TV loss reduces:

* random pixel noise,
* speckled artifacts,
* and unstable textures.

This acts as an edge-preserving denoising method.

---

## 5. Edge Preservation

Sobel edge filtering is used to maintain:

* contours,
* digit strokes,
* and edge consistency.

---

# Final Result

The final system produces:

* recognizable transpose reconstructions,
* improved refined reconstructions,
* higher SSIM values,
* and visually cleaner outputs.

The refinement improves reconstruction quality while still preserving the original transpose attack behavior.

---

# Project Structure

```text
project/
│
├── attack.ipynb
├── CNS Project PPT.pdf
|―― CNS Project Report.pdf
├── README.md
└── data/
```

---

# Requirements

Install the following packages:

```bash
pip install torch torchvision matplotlib scikit-image
```

---

# How to Run the Project

## Step 1 — Clone the Repository

```bash
git clone <your-repository-link>
cd <repository-folder>
```

---

## Step 2 — Install Dependencies

```bash
pip install torch torchvision matplotlib scikit-image
```

---

## Step 3 — Run the cell of the notebook

```bash
python attack.ipynb
```

---

# Expected Output

The program displays:

1. Original MNIST image
2. Raw transpose reconstruction
3. Refined reconstruction

It also prints:

* target class confidence,
* SSIM of transpose reconstruction,
* SSIM of refined reconstruction.

---

# Example Output

```text
Target class: 3
Refined confidence: 0.9987
SSIM vs original - base reconstruction: 0.0361
SSIM vs original - refined reconstruction: 0.0743
```

The refined reconstruction should:

* appear cleaner,
* preserve digit structure,
* and improve SSIM compared to the original transpose output.

---

# Security Implications

This project demonstrates that:

* trained neural networks may unintentionally memorize training data,
* learned weights may leak information,
* and white-box access can expose sensitive patterns.

The attack highlights privacy risks in:

* machine learning systems,
* federated learning,
* cloud-hosted models,
* and publicly shared neural networks.

---

# Defenses Against the Attack

Several defenses can reduce vulnerability:

## Differential Privacy

Adds noise during training to reduce memorization.

## Weight Decay

Limits excessively large weights and reduces overfitting.

## Early Stopping

Stops training before excessive memorization occurs.

## Restricted Model Access

Prevents attackers from accessing model weights.

## Regularization

Dropout and data augmentation improve generalization.

---

# Conclusion

This project successfully implements the transpose attack proposed in NDSS 2024 and extends it with a structural refinement stage.

The refinement improves:

* visual quality,
* structural consistency,
* and SSIM performance,

while preserving the original attack characteristics.

The project demonstrates how neural networks can unintentionally leak training information through their learned parameters.

---

# References

1. Amit Guy et al.
   *Transpose Attack: Stealing Datasets with Bidirectional Training*
   NDSS 2024

2. Official Paper
   [https://www.ndss-symposium.org/ndss-paper/transpose-attack-stealing-datasets-with-bidirectional-training/](https://www.ndss-symposium.org/ndss-paper/transpose-attack-stealing-datasets-with-bidirectional-training/)

3. Official GitHub Repository
   [https://github.com/guyAmit/Transpose-Attack-paper-NDSS24-](https://github.com/guyAmit/Transpose-Attack-paper-NDSS24-)
