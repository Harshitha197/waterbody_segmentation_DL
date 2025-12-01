# Water Body Segmentation from Remote Sensing Images using U-Net

This project focuses on extracting and segmenting water bodies from **Sentinel-2 multi-spectral satellite imagery** using a **U-Net based deep learning model**.  
The study area is **Telangana, India**, and the work spans data acquisition, patch generation, spectral-index-based mask creation, preprocessing, model training, and evaluation.  
The dataset and methods are based on the detailed analysis documented in the project report. :contentReference[oaicite:1]{index=1}

---

## Project Motivation

Water body extraction plays a crucial role in:

- **Urban Planning** — managing lakes, reservoirs, and drainage networks  
- **Agriculture** — irrigation planning and water resource monitoring  
- **Disaster Management** — flood mapping, forecasting, and emergency response  

Deep learning enables accurate, scalable, and automated pixel-level segmentation, outperforming traditional thresholding and classical ML approaches.

---

## Data Acquisition

- **Satellite Source:** Sentinel-2 (ESA)  
- **Band Selection:**  
  - **NIR:** B8 (10m), B8A (20m)  
  - **SWIR:** B11 (20m), B12 (20m)  
- **Study Area:** Telangana, India  
- **Reason:** These bands are ideal for computing water-detection indices like NDWI.  


---

##  Data Preprocessing Pipeline

### 1. **Patch Extraction**
- Extracted 512×512 patches from large TIFF images using a sliding window.  
- Removed patches containing **NaN values** in any band.  
- **Total curated patches:** **4,260**  =

---

### 2. **False Color Composite**
Created composites using:
- **Red:** Band 8  
- **Green:** Band 11  
- **Blue:** Band 12  
with normalization to 0–255 for visualization.  =
---

### 3. **Mask Generation (NDWI + Filtering)**
- Computed **NDWI** for each patch  
- Applied **intensity + spectral filtering**:
  - Dark NIR (bottom 5%)  
  - Dark SWIR (bottom 5%)  
  - High NDWI (> 0.6)  
  - Low-intensity pixels (bottom 3%)  
=

---

### 4. **Morphological Cleanup**
Performed:
- Small-object removal (<100 px)  
- Binary closing & opening with disk-shaped kernels  


---

## Dataset Summary

| Component       | Count |
|----------------|--------|
| Total patches  | **4,260** |
| Image size     | **512×512** |
| Channels       | **3 (False color composite)** |
| Mask values    | **0 = Non-Water, 1 = Water** |


---

## Model Architecture

The model is based on **U-Net**, a popular convolutional encoder–decoder architecture for semantic segmentation.

- Framework: **PyTorch 2.1.0**  
- Input: 512×512 RGB image  
- Output: 512×512 binary mask  
- Loss: **Binary Cross Entropy (BCE)**  
- Optimizer: **Adam**  
- Learning Rate Scheduler: ReduceLROnPlateau  
---

## Training Setup

During each epoch, the following metrics were computed for **both training and validation**:

- **Accuracy**  
- **Dice Score**  
- **IoU (Intersection-over-Union)**  
- **Loss**

---

## Training Curves

The following trends were observed from the plots:

- **IoU** increases steadily and stabilizes around **0.60+**  
- **Pixel accuracy** reaches **98%+**  
- **Dice score** peaks around **0.70+**  
- **Loss decreases consistently**

---

## Results & Test Performance

Sample predictions show accurate segmentation across various regions with different terrain complexities.  

### Key Test Metrics:
- **Pixel Accuracy:** ~98%  
- **IoU:** 0.60–0.68  
- **Dice Score:** 0.70–0.74  
- **Precision:** ~1.00 (perfect water detection with few false positives)  
- **Recall:** ~0.88–0.99 (strong ability to detect water pixels)


