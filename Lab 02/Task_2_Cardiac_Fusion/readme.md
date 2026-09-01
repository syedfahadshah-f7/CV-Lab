# Task 2: Multi-Modal Cardiac Image Fusion

## Data — and why it isn't the raw Kaggle pair

The intended source, the
[Heart CT & MRI Dataset](https://www.kaggle.com/datasets/ziya07/heart-ct-and-mri-dataset),
was downloaded into `CV_DataSets/CT and MRI Fusion/`. Its `synthetic_slices/`
folder, however, turned out to contain only **flat, two-value placeholder
disks** (checked programmatically — every slice is pure background plus one
constant gray value, zero internal texture). Histogram equalization,
thresholding, and fusion on that data would be visually meaningless: nothing
to equalize, nothing structural to fuse.

The same download also includes a `DICOM TO PNG/` folder with real,
detailed CT slices (no matching MRI, though). So this task uses:

- `data/ct_slice.png` — a real CT slice (`0033.IM-0001-0033.dcm.png`),
  chosen for its clear soft-tissue mass and vessel detail.
- `data/mri_slice.png` — a **pixel-aligned simulated MRI-style companion**
  generated from that exact same CT slice (see
  `CV_DataSets` one-off generation notes below), not a real MRI scan.

**Simulation used to build `mri_slice.png`:** within the body region, CT
intensity is inverted (`255 - value`) so dense bone — bright on CT — becomes
low-signal, mimicking how cortical bone appears dark on T1-weighted MRI; a
fractional gamma (`0.8`) lifts soft-tissue midtones; a mild Gaussian blur
softens edges to MRI's characteristically smoother tissue boundaries; light
Gaussian noise mimics acquisition speckle. Background stays black in both
images. This keeps the two "modalities" perfectly pixel-aligned — required
for a meaningful `cv2.addWeighted` fusion — while giving the pipeline real
anatomical structure to work with instead of a blank disk.

If a genuine matched CT/MRI pair becomes available later, drop it into this
`data/` folder under the same filenames and the notebook needs no changes.

## Pipeline (`model_fusion.ipynb`)

1. **Load Modalities** — read both slices as grayscale, assert they're the
   same shape (pixel-aligned).
2. **Histogram Equalization** — applied to each modality independently so
   both contribute full dynamic range to the fusion.
3. **Color Mapping** — CT → `COLORMAP_BONE` (keeps structural edges
   legible); MRI → `COLORMAP_JET` (makes soft-tissue variation stand out).
4. **Multi-Modal Weighted Fusion** — `cv2.addWeighted(ct, 0.7, mri, 0.3, 0)`;
   CT is weighted higher to preserve sharp anatomical boundaries.
5. **Logarithmic + Power-Law Transformations** — applied to the fused
   result (`γ = 0.8`) so the additive blend doesn't crush data into pure
   black or blow it out to pure white.
6. **Comparative Analysis** — CT, MRI, and the fused result shown side by
   side.

Every step's result is saved to `output/` (`01_raw_modalities.png` through
`06_comparative_analysis.png`).

## Running

Open `model_fusion.ipynb`, select the **Python (CV Lab)** kernel, and Run
All. All paths are relative to this folder.
