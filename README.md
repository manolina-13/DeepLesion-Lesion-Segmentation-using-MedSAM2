# # 3D Lesion Segmentation and Evaluation on the DeepLesion Dataset using MedSAM2

## Overview

This notebook performs 3D lesion segmentation on CT scans from the DeepLesion dataset using the MedSAM2 model. Lesion bounding box annotations provided in the dataset are used as prompts for segmentation. The generated masks are compared with available ground-truth annotations, and segmentation performance is evaluated using the Dice Similarity Coefficient (DSC).

The DeepLesion dataset contains 32,735 lesions in 32,120 CT slices from 10,594 studies of 4,427 unique patients. A subset of 5,000 lesions has been manually annotated using a human-in-the-loop MedSAM2 pipeline and is used for segmentation evaluation.

---

## Objectives

- Set up the MedSAM2 framework and required dependencies.
- Download the CT DeepLesion-MedSAM2 dataset.
- Load CT volumes and lesion metadata.
- Perform lesion segmentation using MedSAM2 with bounding-box prompts.
- Generate 3D lesion masks across CT slices.
- Compare predicted masks with ground-truth annotations.
- Evaluate segmentation performance using Dice Score.
- Save segmentation results and visualizations.

---

## Dataset

The notebook uses:

- CT volumes stored in NIfTI (`.nii.gz`) format
- Ground-truth lesion masks
- Metadata file containing:
  - Key slice index
  - Bounding box coordinates
  - Slice range
  - DICOM window settings

Only cases with available ground-truth masks are used for evaluation.

---

## Methodology

### 1. Environment Setup

- Clone the MedSAM2 repository.
- Install required packages.
- Download pretrained MedSAM2 checkpoints.
- Download the CT DeepLesion-MedSAM2 dataset from Hugging Face.

### 2. Data Preprocessing

For each CT volume:

- Apply DICOM windowing using metadata.
- Normalize intensity values to the range [0, 255].
- Convert grayscale slices to RGB format.
- Resize images to 512 × 512 pixels.
- Apply standard image normalization.

### 3. Segmentation

- Extract lesion bounding box information from metadata.
- Use the bounding box on the key slice as the prompt.
- Initialize MedSAM2 inference.
- Propagate segmentation forward through the volume.
- Propagate segmentation backward through the volume.
- Combine results to obtain the final 3D lesion mask.
- Keep the largest connected component as the final prediction.

### 4. Evaluation

For each case:

- Load the corresponding ground-truth mask.
- Compare the predicted mask with the annotation.
- Compute the Dice Similarity Coefficient (DSC):

### Dice Similarity Coefficient (DSC)

The Dice Similarity Coefficient (DSC) is used to measure the overlap between the predicted lesion mask and the ground-truth annotation.

**Formula:**

Dice Score = 2 × (Predicted Mask ∩ Ground Truth Mask) / (Predicted Mask + Ground Truth Mask)

where:

- **A** = Predicted lesion mask
- **B** = Ground-truth lesion mask
- **A ∩ B** = Common pixels shared by both masks

A Dice score of **1.0** indicates perfect overlap, while a score of **0.0** indicates no overlap.

### 5. Visualization

For each evaluated lesion:

- Original CT slice
- Ground-truth mask overlay
- Predicted mask overlay
- Dice score display

### 6. Result Storage

The notebook saves:

- Predicted segmentation masks (`.nii.gz`)
- Comparison figures (`.png`)
- Evaluation results (`.csv`)

---

## Output Files

| File | Description |
|--------|-------------|
| `*_mask.nii.gz` | Predicted lesion mask |
| `*_comparison.png` | Visualization of prediction and ground truth |
| `final_dice_results.csv` | Dice score for each evaluated case |
| `tiny_seg_info202412.csv` | Segmentation metadata and tracking information |

---

## Evaluation Metric

The primary evaluation metric used in this notebook is the Dice Similarity Coefficient (DSC), which measures overlap between predicted and ground-truth lesion masks.

A higher Dice score indicates better segmentation performance.

---

## Workflow

1. Download the DeepLesion-MedSAM2 dataset.
2. Load CT volumes, metadata, and ground-truth masks.
3. Preprocess CT images.
4. Use lesion bounding boxes as prompts.
5. Generate 3D lesion segmentations with MedSAM2.
6. Compare predictions with ground truth.
7. Calculate Dice scores.
8. Save masks, visualizations, and evaluation reports.

---

## Dependencies

- Python
- PyTorch
- MedSAM2
- SimpleITK
- NumPy
- Pandas
- Matplotlib
- Pillow
- scikit-image
- Hugging Face Hub


---

## References

[1] K. Yan, X. Wang, L. Lu, and R. M. Summers, "DeepLesion: Automated Mining of Large-Scale Lesion Annotations and Universal Lesion Detection with Deep Learning," *Journal of Medical Imaging*, vol. 5, no. 3, pp. 036501–036501, 2018.

```bibtex
@article{DeepLesion,
  title={DeepLesion: automated mining of large-scale lesion annotations and universal lesion detection with deep learning},
  author={Yan, Ke and Wang, Xiaosong and Lu, Le and Summers, Ronald M},
  journal={Journal of Medical Imaging},
  volume={5},
  number={3},
  pages={036501--036501},
  year={2018}
}
```

[2] J. Ma, Z. Yang, S. Kim, B. Chen, M. Baharoon, A. Fallahpour, R. Asakereh, H. Lyu, and B. Wang, "MedSAM2: Segment Anything in 3D Medical Images and Videos," *arXiv preprint arXiv:2504.63609*, 2025.

```bibtex
@article{MedSAM2,
    title={MedSAM2: Segment Anything in 3D Medical Images and Videos},
    author={Ma, Jun and Yang, Zongxin and Kim, Sumin and Chen, Bihui and Baharoon, Mohammed and Fallahpour, Adibvafa and Asakereh, Reza and Lyu, Hongwei and Wang, Bo},
    journal={arXiv preprint arXiv:2504.63609},
    year={2025}
}
```
