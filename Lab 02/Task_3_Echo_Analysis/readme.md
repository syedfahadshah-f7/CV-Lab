# Task 3: Real-Time Echocardiogram Video Analysis

## Data

`data/echo_sample.mp4` is one short clip
(`0X1002E8FBACD08477.mp4`, ~100 KB) from the
[Stanford EchoNet-Dynamic Dataset](https://www.kaggle.com/datasets/manojkumarcs28/echonet-dynamic-by-stanford-university).
Copied from the full dataset in the repo-wide `CV_DataSets/Ultrasound Video
Analysis/Videos/` folder (git-ignored, not part of this submission).

## Pipeline (`realtime_echo.ipynb`)

`cv2.VideoCapture` reads `data/echo_sample.mp4` frame by frame (never a
webcam). Every frame goes through the same enhancement chain as Task 1:

1. Histogram equalization — fights murky, low-contrast ultrasound.
2. `COLORMAP_JET` — highlights blood-flow intensity.
3. Gray-world color balance — neutralizes color cast from the colormap.
4. Logarithmic transform — reveals detail inside dark heart chambers.
5. Power-law (gamma = 0.7) transform — suppresses blinding white ultrasound
   backscatter.

Each enhanced frame is concatenated with its raw grayscale counterpart and
shown live via `cv2.imshow` (`SHOW_LIVE = True` at the top of the notebook).
Press `q` in the display window to stop early. If no display is available
(e.g. headless/automated execution), the notebook catches that and keeps
processing every frame without crashing — only the live preview window is
skipped, not the pipeline itself.

Since `cv2.imshow` can't render inside a notebook, a handful of evenly
spaced side-by-side frames are also saved to `output/` as
`frame_0000.png`, `frame_0035.png`, ... so the pipeline's effect is visible
without re-running the live loop.

## Running

Open `realtime_echo.ipynb`, select the **Python (CV Lab)** kernel, and Run
All from this folder. For the live side-by-side window, run it as a normal
interactive notebook (not headless) with a display attached.
