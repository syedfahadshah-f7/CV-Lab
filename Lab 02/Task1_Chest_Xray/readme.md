# Task 1: Diagnostic Enhancement of Chest X-Rays

## Data

`data/sample_xray.png` is one image from the PNEUMONIA class of the
[COVID19+PNEUMONIA+NORMAL Chest X-Ray Image Dataset](https://www.kaggle.com/datasets/sachinkumar413/covid-pneumonia-normal-chest-xray-images),
picked because a pneumonia case best matches the lab's "reveal potential
fluid build-up" scenario. Copied from the full dataset in the repo-wide
`CV_DataSets/Chest X-Rays/PNEUMONIA/` folder (git-ignored, not part of this
submission).

## Pipeline (`xray_enhancement.ipynb`)

1. **Load and Display** — read the sample as a single-channel grayscale matrix.
2. **Contrast Enhancement** — `cv2.equalizeHist` redistributes intensities
   across the full range; raw vs. equalized images and histograms are
   compared side by side.
3. **Color Mapping** — the equalized image is mapped through
   `COLORMAP_JET` to make subtle intensity differences visually distinct.
4. **Color Balance** — a gray-world white balance (per-channel mean
   matching) neutralizes any color cast the colormap introduced.
5. **Color Filtering (Thresholding)** — pixels above the 90th percentile of
   equalized intensity are kept, isolating only the densest tissue (bone /
   dense fluid).
6. **Logarithmic Transformation** — applied to the raw image to expand dark
   background regions and reveal faint rib edges.
7. **Power-Law (Gamma) Transformation** — `γ = 0.6` applied to the raw image
   brightens midtones, softening bone contrast so soft tissue reads more
   clearly.

Every step's result is saved to `output/` as it runs (`01_raw.png` through
`07_gamma_transform.png`).

## Running

Open `xray_enhancement.ipynb`, select the **Python (CV Lab)** kernel (see
the setup instructions in `Lab 02/readme.md`), and Run All. All paths are
relative, so it only needs to be run from this folder.
