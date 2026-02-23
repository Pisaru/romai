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

Short demo preview is placed in `docs/demo.gif`. If you prefer a video file, `docs/demo.mp4` may be used and linked in this README.

Embedded preview (if `docs/demo.gif` exists):

![ROMAI Demo](docs/demo.gif)

Full demonstration video (hosted):
[Watch Full Demo on YouTube](https://youtu.be/LVpVHAe3rAk)

## Validation Results

The validation summaries below reflect the core results used to evaluate ROMAI. They present the main reliability and agreement metrics versus the universal goniometer and a consumer IMU system.

### Sample & Protocol

* Sample: healthy adults, n = 40
* Protocol: each movement repeated 3 times (end-range hold 3 s); simultaneous measurements with universal goniometer (reference), IMU, and video-based ROMAI.

### Within-method (intra-device) reliability — Pose estimation (ICC₂,₃)

| Joint (shoulder) | ICC₂,₃ |
| ---------------: | :----: |
|          Flexion |  0.968 |
|        Extension |  0.984 |
|        Abduction |  0.976 |

* SEM (pose estimation): ~0.73–1.05°
* MDC₉₅ (pose estimation): ~2.0–2.9°

### Between-method reliability (Goniometer vs Pose Estimation) — ICC₂,₁

* ICC₂,₁ range: **0.812 – 0.939** (movement-dependent; highest at extension: 0.939)
* Interpretation: overall good to excellent agreement between ROMAI and the universal goniometer.

### Correlation (Goniometer vs Pose Estimation)

* Pearson r range: **0.593 – 0.646** (all p < .01)
* Interpretation: moderate to strong linear association.

### Bland–Altman (Goniometer vs Pose Estimation)

* Mean difference (bias): **−1.03° to 3.32°** (movement-dependent)
* 95% Limits of Agreement (LoA) width: approximately **±10–12°**
* Interpretation: small average bias; LoA acceptable for group-level inference but individual differences may reach ±10°.

Combined Bland–Altman figure (Flexion / Extension / Abduction) is available as a single image at `docs/bland_altman.png`.

![Bland–Altman figure including Flexion / Extension / Abduction](docs/bland_altman.png)

### Goniometer vs IMU (summary)

* ICC₂,₁ range: **0.343 – 0.873** (movement and joint dependent)
* Example Bland–Altman for flexion: mean difference ≈ **24.60°** (systematic discrepancy)
* Pearson r: **0.276 – 0.449** (generally low, p < .05)
* Interpretation: IMU and goniometer showed substantial method differences under the current configuration and sensor placement.

### Regression (goniometer as reference)

* Pose estimation was a statistically significant predictor for all shoulder movements (p < .001).
* Standardized β: **0.36 – 0.62**
* Example unstandardized effect: 1° increase in pose-estimated angle corresponds approximately to **0.36°** (flexion), **0.50°** (extension), **0.60°** (abduction) increase in goniometer measurement.

## Interpretation (summary)

* AI-based 2D pose estimation (ROMAI) demonstrated **excellent repeatability** and **acceptable agreement** with the universal goniometer, supporting its potential as a clinical adjunct for shoulder ROM assessment.
* Under the tested conditions, commercial IMU motion-capture showed notable discrepancies relative to the goniometer, likely reflecting calibration and placement sensitivity.
* Bland–Altman LoA (~±10–12°) indicate suitability for group-level analysis and monitoring; for individual clinical decisions, apply SEM/MDC thresholds.

## System Architecture

Pipeline workflow:

1. Video input (smartphone 1080p/60fps, fixed tripod)
2. Human detection → HRNet-W32 (MMpose top-down) → 17 keypoints extraction
3. Joint angle calculation via anatomical vectors (e.g., trunk vector vs humeral vector)
4. Batch processing → per-video keypoint & angle CSVs → visualization video (optional)

**Notebook output conventions (MMpose_Rt_Shld.ipynb):**

* `{video_name}_alljt.csv` — framewise keypoints + raw angle computations
* `{video_name}_rt_shld.csv` — processed per-video right-shoulder summary

Place per-video CSVs in `results/raw/` and processed summaries in `results/processed/`.

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

Prepare videos in your working directory or mount Google Drive.

Run (example):

```bash
python run_pose_estimation.py --input your_video.mp4 --output results/processed/
```

Typical outputs:

* `results/raw/{video_name}_alljt.csv` — framewise keypoints + raw angles
* `results/processed/{video_name}_rt_shld.csv` — processed per-video right-shoulder summary
* `results/validation_summary.csv` — joint-wise summary if generated by scripts
* `docs/bland_altman.png` — combined Bland–Altman figure (Flexion/Extension/Abduction)
* `docs/demo.*` — demo GIF or MP4 used for README preview

## Recommended Repository Structure

```
ROMAI/
├─ README.md
├─ MMpose_Rt_Shld.ipynb
├─ requirements.md
├─ LICENSE
├─ .gitignore
├─ docs/
│  ├─ demo.gif
│  └─ bland_altman.png
```

(Optionally add `scripts/` and `results/` if you will share analysis scripts and CSV outputs.)

## Reproducibility & Materials

* Include raw per-subject CSVs and an analysis script (`scripts/validation_analysis.py`) if you want exact reproducibility for ICC/Bland–Altman/regression outputs.
* Do not include manuscript drafts (e.g., `작업중.pdf`) in the public README; keep them private if needed for submission.

## Limitations

* Sample limited to healthy adults; external validation in clinical populations required.
* 2D methods are sensitive to camera angle and occlusion.
* LoA breadth requires SEM/MDC-informed interpretation for single-subject decisions.

## Citation

If you use this repository in academic work, please cite:

```
Author(s). Reliability and Validity of Artificial Intelligence–Based Pose Estimation in Measuring Joint Range of Motion. Journal Name. Year.
```

(Replace with final publication details and DOI when available.)

## License

MIT License
