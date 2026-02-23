# ROMAI

**Reliability and Validity of Artificial Intelligence–Based Pose Estimation in Measuring Joint Range of Motion**

ROMAI is a research-based automated 2D human pose estimation and joint angle analysis pipeline built on OpenMMLab’s MMpose framework. It extracts human keypoints from video and computes joint range of motion (ROM) automatically. This repository accompanies a validation study evaluating reliability and agreement against reference measurement methods.

## Project Overview

ROMAI provides:

* 2D human pose estimation
* Automated joint angle computation
* Batch video processing (per-video outputs)
* Google Colab execution support
* Research-validated measurement performance (see Validation Results)

Target applications:

* Clinical ROM assessment
* Rehabilitation outcome monitoring
* Sports biomechanics
* Research reproducibility

## Demo

### Pose Estimation + Angle Overlay

(Short demo placed in `docs/`)

If you want an animated preview inside README, use a short GIF (`docs/demo.gif`). If you uploaded a short MP4 instead, link to it from README or use the GIF thumbnail approach.

Embedded preview (if you keep `docs/demo.gif`):

```markdown
![ROMAI Demo](docs/demo.gif)
```

Full demonstration video (hosted):
[Watch Full Demo on YouTube](https://youtu.be/LVpVHAe3rAk)

## Validation Results

The validation study and numerical results are summarized below (detailed methods and statistics in the manuscript and `작업중.pdf`). 

### Sample & General Metrics

* Sample: healthy adults, n = 40
* Protocol: each movement repeated 3 times (end-range hold 3 s); simultaneous measurements with universal goniometer, IMU, and video

### Within-method (intra-device) reliability — Pose estimation (ICC₂,₃)

| Joint (shoulder) | ICC₂,₃ |
| ---------------: | :----: |
|          Flexion |  0.968 |
|        Extension |  0.984 |
|        Abduction |  0.976 |

* SEM range (pose estimation): ~0.73–1.05°
* MDC₉₅ (pose estimation): ~2.0–2.9°

### Between-method reliability (Goniometer vs Pose Estimation) — ICC₂,₁

* Range: **0.812 – 0.939** (movement-dependent; highest at extension: 0.939)
* Interpretation: overall good to excellent agreement

### Correlation (Goniometer vs Pose Estimation)

* Pearson r range: **0.593 – 0.646** (all p < .01)
* Interpretation: moderate to strong linear association

### Bland–Altman (Goniometer vs Pose Estimation)

A combined Bland–Altman figure including Flexion / Extension / Abduction panels is available as a single image: `docs/bland_altman.png`.

* Mean difference (bias): **−1.03° to 3.32°** (movement-dependent)
* 95% Limits of Agreement (LoA) width: approx **±10–12°**
* Interpretation: small average bias; LoA acceptable for group-level inference but individual differences may reach ±10°.

(If you prefer separate per-joint images, add `docs/bland_altman_flexion.png` etc.; currently all three are combined in `docs/bland_altman.png`.)

### Goniometer vs IMU (summary)

* ICC₂,₁ range: **0.343 – 0.873** (movement and joint dependent)
* Example Bland–Altman for flexion: mean difference ≈ **24.60°** (systematic discrepancy)
* Pearson r: **0.276 – 0.449** (generally low, p < .05)
* Interpretation: IMU and goniometer showed substantial method differences under current configuration.

### Regression (goniometer as reference)

* Pose estimation was a statistically significant predictor for all shoulder movements (p < .001).
* Standardized β: **0.36 – 0.62**
* Example unstandardized effect: 1° increase in pose-estimated angle corresponds approximately to **0.36°** (flexion), **0.50°** (extension), **0.60°** (abduction) increase in goniometer measurement.

## Interpretation (summary)

* AI-based 2D pose estimation demonstrated **excellent repeatability** and **acceptable agreement** with the universal goniometer, supporting its potential utility as a clinical adjunct tool.
* IMU-based motion capture exhibited notable discrepancies in this study, likely related to sensor placement and calibration.
* Bland–Altman LoA (~±10–12°) suggests caution for single-subject clinical decision-making near diagnostic thresholds; apply SEM/MDC for interpretation of individual changes.

## System Architecture

Pipeline workflow:

1. Video input (smartphone 1080p/60fps, fixed tripod)
2. Human detection → HRNet-W32 (MMpose top-down) → 17 keypoints extraction
3. Joint angle calculation via anatomical vectors (e.g., trunk vector vs humeral vector)
4. Batch processing → per-video keypoint & angle CSVs → visualization video (optional)

**Note about per-video outputs (from processing notebooks / scripts):** the analysis notebook produces per-video CSVs named like:

* `{video_name}_alljt.csv` — all-joints CSV (raw per-frame / per-keypoint extraction)
* `{video_name}_rt_shld.csv` — right-shoulder processed CSV (per-video joint-angle results)
  These are created in local result folders by the notebook (`local_alljt_csv` / `local_done_csv` variables in `MMpose_Rt_Shld.ipynb`). Keep per-video CSVs under `results/raw/` and processed CSVs under `results/processed/` (example structure below).

## Installation (Google Colab)

Run the following in the first cell:

```bash
# PyTorch (CUDA 11.8 build)
!pip install torch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 --index-url https://download.pytorch.org/whl/cu118

!pip uninstall -y jax jaxlib transformers accelerate torchao

!pip install numpy==1.24.4 pandas==1.5.3 scipy==1.10.1 requests==2.28.2 tqdm==4.65.2 filelock==3.14.0
!pip install transformers==4.30.2

!pip install -U openmim
!mim uninstall mmengine mmcv mmdet mmpose -y
!mim install mmengine
!pip install mmcv==2.0.1 -f https://download.openmmlab.com/mmcv/dist/cu118/torch2.0/index.html
!mim install "mmdet<3.3.0"
!mim install "mmpose>=1.1.0"

!git clone https://github.com/open-mmlab/mmpose.git
%cd mmpose
!pip install -r requirements.txt
!pip install -v -e .
%cd /content
```

After installation, restart the runtime.

## Usage

Prepare videos in your working directory (or mount Google Drive).

Run (example):

```bash
python run_pose_estimation.py --input your_video.mp4 --output results/processed/
```

Outputs (examples):

* `results/raw/{video_name}_alljt.csv` — framewise keypoints + raw angles
* `results/processed/{video_name}_rt_shld.csv` — processed per-video right-shoulder summary
* `results/validation_summary.csv` — joint-wise summary (if generated by scripts)
* `docs/bland_altman.png` — combined Bland–Altman figure (Flexion/Extension/Abduction)
* `docs/demo.*` — demo GIF or MP4 placed in `docs/` for README preview

## Recommended Repository Structure (updated to reflect notebook outputs)

```
ROMAI/
├─ README.md
├─ run_pose_estimation.py
├─ requirements.txt
├─ notebooks/
│  └─ MMpose_Rt_Shld.ipynb         # processing & per-video CSV generation (produces *_alljt.csv, *_rt_shld.csv)
├─ scripts/
│  └─ validation_analysis.py       # summary statistics / Bland–Altman / ICC generation
├─ docs/
│  ├─ demo.gif  (or demo.mp4)      # short preview/gif or video for README
│  └─ bland_altman.png             # combined 3-panel Bland–Altman figure (flex/ext/abd)
├─ results/
│  ├─ raw/                         # per-video raw CSVs (e.g., {video}_alljt.csv)
│  └─ processed/                   # per-video processed CSVs (e.g., {video}_rt_shld.csv)
└─ data/                           # if sharing small sample videos or anonymized examples
```

* Keep the single `docs/bland_altman.png` (combined figure) as you have now — README references that single file.
* Per-video CSV naming follows the notebook conventions (`{video_name}_alljt.csv`, `{video_name}_rt_shld.csv`): place them under `results/raw/` and `results/processed/` respectively.

## Reproducibility & Materials

* Include raw per-subject CSVs (reference goniometer / IMU / ROMAI) and the statistical analysis scripts used to compute ICC, Pearson r, MAE/RMSE, and Bland–Altman plots (`scripts/validation_analysis.py`).
* Document camera settings, participant clothing, IMU placement, and preprocessing steps in `METHODS.md`. The study methods and stats are available in the working manuscript (see `작업중.pdf`). 

## Limitations

* Sample limited to healthy adults; generalization to older or pathological populations is not guaranteed.
* 2D video-based methods are sensitive to camera angle and occlusion.
* LoA breadth indicates careful interpretation for individual clinical decisions.

## Citation

If you use this repository in academic work, please cite:

```
Author(s). Reliability and Validity of Artificial Intelligence–Based Pose Estimation in Measuring Joint Range of Motion. Journal Name. Year.
```

(Replace with final publication details and DOI when available.)

## License

MIT License

---
