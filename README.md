                       image_pixel_manipulator and medical image artifact detection and restoration using matrix mathematics
A NumPy-based image manipulation toolkit using synthetic RGB images 
an image manipulation toolkit using 3D NumPy array slicing, dtype casting (uint8/float32) and boolean masks for cropping, flipping, channel extraction and brightness control



    # Medical Image Artifact Detection and Restoration Using Matrix Mathematics
Overview
Medical imaging techniques such as Magnetic Resonance Imaging (MRI) and Computed Tomography (CT) are widely used to examine internal body structures. However, medical images can sometimes contain artifacts, such as unwanted bright streaks, dark regions, or signal distortions, which may affect image quality.
Traditional smoothing and blurring techniques can reduce noise but may also remove important anatomical details, including tissue boundaries and tumor margins.
This project implements a **pixel-level image artifact detection and restoration algorithm using NumPy and mathematical matrix operations**. It identifies pixels matching predefined corruption patterns and reconstructs them using the values of healthy neighboring pixels while attempting to preserve surrounding image details.

## Problem Statement

Artifacts in medical images can interfere with visual interpretation and reduce image quality. Applying global smoothing may unintentionally alter healthy regions.
The objective of this project is to develop a localized restoration technique that:
- Detects potentially corrupted pixels.
- Creates a binary mask to distinguish damaged and healthy pixels.
- Examines a local neighborhood around each damaged pixel.
- Reconstructs damaged pixels using healthy neighboring values.
- Evaluates restoration quality using Peak Signal-to-Noise Ratio (PSNR).

## Objectives
1. Implement image processing using array and matrix operations.
2. Detect predefined black and white corruption patterns.
3. Use local neighborhood-based interpolation to reconstruct damaged pixels.
4. Minimize unnecessary modifications to unaffected regions.
5. Quantitatively evaluate image restoration using PSNR.

## Technologies Used

- **Python** — Core programming language.
- **NumPy** — Vectorized array operations and mathematical computations.
- **Matplotlib** — Image visualization and comparison.
- **PSNR** — Quantitative image reconstruction quality metric.

## Methodology

The algorithm follows three main steps.

### Step 1: Damage Detection and Binary Mask Generation

The input image is represented as a numerical array of pixel values.

For an 8-bit RGB image, each pixel contains three channel values ranging from 0 to 255.

The algorithm identifies pixels matching predefined corruption patterns:

- Black pixel: `[0, 0, 0]`
- White pixel: `[255, 255, 255]`

A binary detection mask is generated:

- `True` — Pixel matches a predefined corruption pattern.
- `False` — Pixel does not match the predefined pattern.

This mask identifies candidate pixels for restoration.

**Important:** Black and white pixels are not always artifacts in real medical images. This detection strategy relies on an assumption that must be validated for the input dataset.

### Step 2: Adaptive Local Neighborhood Slicing

For every detected pixel, the algorithm examines a local neighborhood of size **3 × 3 pixels**.

The neighborhood provides spatial information from the surrounding image.

Instead of processing the entire image with a blur filter, the algorithm focuses on the selected pixel and its immediate surroundings.

This localized approach helps avoid unnecessary changes to pixels outside the detected regions.

### Step 3: Noise-Filtered Color Interpolation

The algorithm uses the binary mask to exclude neighboring pixels that have also been identified as damaged.

It then calculates the average RGB values of the remaining healthy neighbors.

For a damaged pixel at position \((i,j)\), the reconstructed value is:

\[
I_{\text{restored}}(i,j)=
\frac{1}{N}\sum_{k=1}^{N}I_k
\]

Where:

- \(I_{\text{restored}}(i,j)\) is the reconstructed pixel value.
- \(I_k\) represents a healthy neighboring pixel.
- \(N\) is the number of valid neighboring pixels.

The calculated value replaces the damaged pixel, helping it blend with the surrounding image.

If no valid neighbors are available, a separate fallback strategy is required.

## Algorithm Workflow

1. Load the input image as a NumPy array.
2. Identify candidate corrupted pixels.
3. Generate a binary damage mask.
4. Find the local 3 × 3 neighborhood of each candidate pixel.
5. Exclude neighbors marked as damaged.
6. Calculate the average of the remaining valid neighbors.
7. Replace the candidate pixel with the reconstructed value.
8. Compare the original, corrupted, and restored images.
9. Calculate PSNR to evaluate restoration quality.

## Mathematical Evaluation: PSNR

Peak Signal-to-Noise Ratio (PSNR) is a commonly used metric for evaluating image reconstruction quality.

It is calculated using Mean Squared Error (MSE):

\[
MSE=\frac{1}{MN}\sum_{i=1}^{M}\sum_{j=1}^{N}
\left(I(i,j)-K(i,j)\right)^2
\]

For RGB images, the error must also account for the color channels.

PSNR is defined as:

\[
PSNR=10\log_{10}\left(\frac{MAX^2}{MSE}\right)
\]

Where:

- \(MAX\) is the maximum possible pixel intensity, typically 255 for 8-bit images.
- \(MSE\) measures the average squared difference between corresponding pixel values.
- \(M\) and \(N\) represent the image dimensions.

A higher PSNR generally indicates greater similarity to the reference image.

### Reported Result

The current project reports a **PSNR improvement of 1.80 dB** after restoration.

This result should be interpreted as an improvement in numerical image similarity, provided that the PSNR values were calculated against the same clean reference image before and after restoration.

Actual values, test-image details, and evaluation conditions should be documented to make the result reproducible.

PSNR alone does not establish clinical accuracy or prove that tumor boundaries and other anatomical structures remain unchanged.

## Advantages

- Uses mathematical matrix operations rather than relying on a ready-made restoration function.
- Targets selected pixels instead of globally smoothing the image.
- Uses neighboring healthy pixels to estimate missing information.
- Provides a quantitative evaluation method through PSNR.
- Offers an educational demonstration of image masks, local neighborhoods, interpolation, and numerical image processing.

## Limitations

- Exact black and white pixel detection may incorrectly classify legitimate image content.
- A 3 × 3 neighborhood may not be sufficient for large or continuous artifacts.
- Averaging can introduce errors near sharp edges or anatomical boundaries.
- The method does not automatically distinguish every type of MRI or CT artifact.
- Clinical suitability requires testing on representative medical images and validation by qualified medical professionals.

## Future Improvements

- Implement more robust artifact detection using local intensity statistics.
- Support larger and adaptive neighborhood windows.
- Compare mean-based interpolation with median-based and edge-aware reconstruction.
- Evaluate performance using PSNR, Structural Similarity Index (SSIM), and edge-preservation metrics.
- Test on publicly available medical imaging datasets with appropriate reference images.
- Assess whether restoration preserves clinically relevant structures and diagnostic information.

## Usage

1. Open the project in your Python environment or Google Colab.
2. Load the input image.
3. Run the damage detection and restoration algorithm.
4. Visualize the original, corrupted, and restored images.
5. Calculate PSNR using a clean reference image.
6. Record the evaluation results.

The exact execution command depends on the filenames and implementation used in the repository.

## Applications

Potential educational and research applications include:

- Medical image processing.
- Pixel-level image reconstruction.
- Localized noise and artifact removal experiments.
- Numerical image analysis.
- Evaluation of interpolation techniques.

**Disclaimer:** This project is an experimental image-processing implementation and is not a clinically validated diagnostic tool. Restored images should not be used to make medical decisions without appropriate validation.

## Conclusion

This project demonstrates how binary masks, neighborhood slicing, and local interpolation can be combined to reconstruct selected corrupted pixels using mathematical matrix operations.

By targeting candidate damaged pixels and evaluating reconstruction quality with PSNR, the project provides a foundation for studying localized image restoration while highlighting the importance of preserving anatomical details and validating results on realistic medical images.

