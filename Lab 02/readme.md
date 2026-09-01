# Lab 02: Medical Imaging Portfolio

Three independent OpenCV/NumPy pipelines for medical image enhancement, built
per `Lab 02 Tasks.pdf`.

| Task | Folder | Notebook | Modality |
|---|---|---|---|
| 1 | `Task1_Chest_Xray/` | `xray_enhancement.ipynb` | Chest X-Ray |
| 2 | `Task_2_Cardiac_Fusion/` | `model_fusion.ipynb` | CT + MRI fusion |
| 3 | `Task_3_Echo_Analysis/` | `realtime_echo.ipynb` | Ultrasound video |

## Setup

All three labs in this repository share one virtual environment at the repo
root (`d:\Sem 7\CV Lab\.venv`), so it only needs to be created once:

```bash
cd "d:\Sem 7\CV Lab"
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
python -m ipykernel install --user --name=cv-lab --display-name "Python (CV Lab)"
```

Open any notebook in this repo and select the **Python (CV Lab)** kernel.
Each task's own `requirements.txt` (in this folder) lists just the packages
this submission actually needs, for a reviewer rebuilding only this portion.

## Data

Every notebook reads only from its own task's `data/` folder using relative
paths (e.g. `data/sample_xray.png`) — no absolute paths, per the submission
guidelines. Those `data/` folders hold only the small sample files each
script actually uses.

The full source datasets (COVID/Pneumonia/Normal chest X-rays, the heart
CT/MRI dataset, and EchoNet-Dynamic ultrasound videos) live in the
repo-wide, git-ignored `CV_DataSets/` folder one level up, and are never
committed — see each task's own `readme.md` for exactly which sample was
pulled from there and why.

## Running

Run each notebook top to bottom from its own task folder (e.g. open
`Task1_Chest_Xray/xray_enhancement.ipynb` and "Run All"). Each notebook
creates its `output/` folder automatically and saves every enhanced view
there as it runs.
