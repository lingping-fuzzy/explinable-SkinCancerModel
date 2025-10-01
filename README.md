# explinable-SkinCancerModel
# Extracted Features

We extract features from the segmented image, encompassing multiple specialized categories. The implementation will be publicly available through: [https://github.com/lingping-fuzzy/explinable-SkinCancerModel](https://github.com/lingping-fuzzy/explinable-SkinCancerModel) after acceptance.

The extracted features categories are as follows:

1. **Texture & Spatial Features** – Capturing local intensity variations, contrast, and edge density to characterize lesion texture patterns.
2. **Morphological & Geometric Features** – Quantifying shape-related properties such as asymmetry, compactness, and convex hull ratios.
3. **Vascular & Physiological Features** – Assessing blood flow indices and oxygen consumption estimates to infer the vascularity of the lesion.
4. **Summary Indices** – Aggregated statistical measures summarizing key feature distributions.
5. **Color Variance Features** - Evaluating variations in red, blue, and overall color intensity throughout the lesion.
6. **Frequency & Spectral Features** – Extracting high- and low-frequency components to analyze the structure of the lesion in the spectral domain.
7. **CNN Feature Embeddings** - Deep feature representations obtained from the convolutional layers of the HSNN model.
8. **Colorimetric Features** - Measurement of color-based attributes relevant to lesion classification.
9. **Histopathological Similarity Characteristics** - Compare the lesion characteristics with known malignant and benign cases based on histopathological patterns.

![Example extracted feature statistical analysis – Density plots for three individual features, grouped by skin cancer type.](fig_violinplot_asymmetry_index.png)

![Example extracted feature statistical analysis – Density plots for three individual features, grouped by skin cancer type.](fig_density_LBP_Variance.png)

*Figure 1: Example extracted feature statistical analysis – Density plots for three individual features, grouped by skin cancer type.*

The extracted statistical analysis of features is presented in Figure 1, which includes a violin plot and density plots, illustrating the distribution of features in seven types of skin cancer from the HAM10000 dataset. The top panel features a violin plot with kernel density estimation, showing the distribution of asymmetry index (0.1 to 0.9) and edge density (0.3 to 1.0) grouped by cancer type (bkl, akiec, mel, df, bcc, vasc, nv). This violin plot integrates a boxplot with a rotated kernel density plot, offering insights into the data spread and central tendency for each feature across the classes. The bottom panel presents density plots for LBP variability (peak at 4.0), grouped by cancer type, highlighting variation and overlap in feature distributions. This analysis helps to solve the classification problem by assessing the discriminative power of these individually extracted features, derived through various image processing methods, to distinguish between the seven classes of skin cancers.

## Feature Summary Tables

The following tables summarize the characteristics of the selected features used in our study for the detection of skin cancer, organized by clinical categories aligned with the ABCDE criteria (Asymmetry, Border, Color, Diameter, and Evolving) and additional categories such as texture and network-based characteristics.

### Table 1: ABCDE Rule-based Features

**Clinical Category** | **Feature Name** | **Description**
---------------------|------------------|-----------------
**Asymmetry (A)** | Asymmetry Index | Difference between image and its flipped version, normalized by sum of pixel intensities
**Asymmetry (A)** | Eccentricity | $\sqrt{1 - \left(\frac{\text{minor axis}}{\text{major axis}}\right)^2}$, measures elongation (0 for circles, ~1 for lines)
**Asymmetry (A)** | Solidity | Area / Convex Hull Area, lower values indicate irregular shapes
**Border (B)** | Border Irregularity | Variance of Canny edge intensities, scaled by 1000
**Border (B)** | Edge Density | Percentage of edge pixels from Canny edge detection
**Border (B)** | Directional Variance | Variance of HOG features, indicating variation in edge orientations
**Border (B)** | Edge Roughness (Fourier) | Sum of high-magnitude FFT components, capturing spectral edge irregularity
**Border (B)** | Branching Complexity | Variance of skeletonized edges, scaled by 100, reflecting edge pattern complexity
**Color (C)** | Mean Red Intensity | Mean intensity of the red channel in the image
**Color (C)** | Mean Blue Intensity | Mean intensity of the blue channel in the image
**Color (C)** | Color Variability | Standard deviation across all color channels
**Color (C)** | Color Variance 1-8 | Regional color variance (channel/region 1-8)
**Diameter (D)** | Area | Contour area from `cv2.contourArea`, proxy for lesion size
**Diameter (D)** | Perimeter | Contour perimeter from `cv2.arcLength`, related to lesion size
**Diameter (D)** | Circularity | $4\pi \cdot \text{Area} / \text{Perimeter}^2$, measures how circular the lesion is (1 for perfect circle)
**Diameter (D)** | Compactness Index | Sum of pixel intensities / $\sqrt{\text{image area}}$, relates intensity to size
**Evolving (E)** | Predicted Shape Change | Variance-based estimate of border deformation over time (scaled by 0.18)
**Evolving (E)** | Estimated Malignant Transformation Probability | Average of oxygen consumption index and shape change rate, indicating progression risk

### Table 2: Image Analysis-based Features

**Clinical Category** | **Feature Name** | **Description**
---------------------|------------------|-----------------
**Texture** | Contrast | GLCM contrast, measuring intensity differences between neighboring pixels
**Texture** | Dissimilarity | GLCM dissimilarity, measuring local intensity variations
**Texture** | Homogeneity | GLCM homogeneity, measuring closeness of pixel intensities
**Texture** | Energy | GLCM energy, measuring uniformity of intensity distribution
**Texture** | Correlation | GLCM correlation, measuring linear dependency of intensities
**Texture** | Entropy | Entropy of HOG features, indicating texture complexity
**Texture** | Fractal Dimension | Box-counting fractal dimension, capturing texture complexity
**Texture** | Lacunarity | Variance / mean of grayscale image, measuring texture heterogeneity
**Texture** | LBP Mean | Mean of Local Binary Pattern values, capturing local texture patterns
**Texture** | LBP Variance | Variance of Local Binary Pattern values, indicating texture variability
**Texture** | texture_value | Summary index of texture features (calculation not specified)
**Frequency/Spectral** | High-Frequency Energy | Sum of absolute wavelet coefficients (cH, cV, cD), capturing high-frequency components
**Frequency/Spectral** | Low-Frequency Energy | Sum of absolute approximation coefficients (cA), capturing low-frequency components
**Frequency/Spectral** | TotalPower | Total power from FFT or wavelet analysis (calculation not specified)
**Frequency/Spectral** | meanpower | Mean power from FFT or wavelet analysis (calculation not specified)
**Frequency/Spectral** | freqencyEntropy | Entropy of frequency components (calculation not specified)
**Frequency/Spectral** | highFreqpowerRatio | Ratio of high-frequency power (calculation not specified)
**Frequency/Spectral** | Dominant Frequency Component | Mean of FFT magnitude, indicating dominant frequency
**Vascular/Physiological** | Vascular Density | Sum of skeletonized edges / image area, indicating vascular structure density
**Vascular/Physiological** | Estimated Blood Flow Index | Mean pixel intensity / 255, estimating blood flow
**Vascular/Physiological** | Estimated Oxygen Consumption Index | Entropy of flattened image, estimating metabolic activity
**CNN-Based** | Feature Cluster Compactness | Mean absolute deviation of ResNet50 feature embeddings
**CNN-Based** | Feature Variance | Variance of ResNet50 feature embeddings
**Histopathological** | Histopathological Similarity to Malignant Cases (M) | Cosine similarity to malignant histopathology features (placeholder database)
**Histopathological** | Histopathological Similarity to Benign Cases | 1 - malignant similarity, indicating benign feature similarity
**Demographics** | age | Patient age (metadata, not calculated)
**Demographics** | sex | Patient sex (metadata, not calculated)
**Demographics** | localization | Lesion location on body (metadata, not calculated)
**Summary Indices** | edge_value | Summary index of edge-related features (calculation not specified)

## Dataset Considerations

The HAM10000 dataset, while valuable for training AI models to detect skin cancer, presents inherent limitations that hinder its ability to provide the comprehensive information dermatologists require for a definitive diagnosis using the ABCDE criteria: asymmetry, border irregularity, color variation, diameter greater than 6mm, and evolution. Factors such as image quality, lighting conditions, angles, and resolution often obscure critical details, while essential metrics like Breslow depth, vital for melanoma staging, necessitate a biopsy and cannot be derived from images alone, leaving the dataset as a partial source of diagnostic clues rather than a complete severity assessment tool.

To address this, the proposed AI and machine learning algorithms aim to maximize the extraction of features from the available limited data, focusing on attributes aligned with dermatological interests, such as asymmetry and color variation, to enhance diagnostic support. Although these models achieve high accuracy comparable to dermatologists, their susceptibility to false positives and negatives underscores their role as an assistive tool rather than a replacement for professional judgment, driving efforts to enrich the feature set for more robust clinical insights.

This structured approach ensures a comprehensive feature set, integrating diverse perspectives from image segmentation, deep learning, and metadata analysis to improve interpretability and predictive accuracy. **A total of 62 features have been extracted.**
