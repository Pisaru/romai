# **Reliability and Validity of Artificial Intelligence–Based Pose Estimation in Measuring Joint Range of Motion**

This repository provides an adapted 2D human pose estimation and joint angle analysis pipeline based on OpenMMLab’s MMpose framework (v1.3.2; an open-source toolbox for pose estimation using deep learning models such as PyTorch) with the HRNet-W32 architecture, refined for clinical validation. It extracts human keypoints from standard 2D video and automatically computes joint range of motion (ROM).

This repository accompanies a clinical validation study evaluating the intra- and inter-instrument reliability and concurrent validity of this AI-based approach against the universal goniometer (Universal standard) and a commercial IMU-based motion capture system (Noraxon myoMOTION).

## Project Overview

This repository presents an academic project implementing a real-time joint angle measurement pipeline based on MMPOSE.

The pipeline is optimized for use in Google Colab and includes:
* 2D markerless human pose estimation using a pre-trained HRNet-W32 model
* Automated joint angle computation from extracted keypoints
* Batch video processing that outputs raw CSV keypoint data and angle-annotated videos
* Experimental evaluation for shoulder range of motion (ROM) (flexion, extension, abduction)

## Demo

![ROMAI Demo](docs/demo.gif)

*(Demonstration of automated joint tracking and angle overlay during shoulder movement.)*

## Validation Results (Summary)

Measurement performance is based on a comparative study of 40 healthy adults (23 males, 17 females). Shoulder movements were evaluated with a 3-second terminal hold.

* **Within-Instrument Reliability (AI Pose Estimation):** * Excellent intra-session reliability across movements.
  * ICC: .887 – .991
  * SEM: 0.73° – 1.22°
* **Inter-Method Agreement (AI vs. Goniometer):** * Good to excellent agreement (ICC₂,₁ = .812 – .939).
  * Superior to the agreement between the IMU and goniometer.
  * Bland–Altman mean bias ranged from −1.03° to 3.32°.
* **Concurrent Validity:** AI-based pose estimation significantly predicted goniometric ROM across all tested movements (p < .001, Standardized β = .36 – .62).

*The proposed MMPOSE-based pipeline demonstrated high repeatability and acceptable agreement with a universal goniometer, suggesting its potential utility as a contact-free method for objective shoulder ROM assessment in research and clinical contexts.*

## Repository Structure

```text
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

## Installation & Usage (Google Colab)

All execution logic, environment setup, and processing are contained within the `MMpose_Rt_Shld.ipynb` notebook. It is designed to run in a Google Colab environment with a GPU runtime (e.g., T4).

1. Upload your input videos (`.mp4`) to a folder in your Google Drive.
2. Open `MMpose_Rt_Shld.ipynb` in Google Colab and ensure the runtime is set to GPU.
3. In the first cell, update the `MY_PATH` variable to match your Google Drive folder path:
```python
# Only change this variable for your own path!
MY_PATH = "sample_img/action_video" 

```


4. Run the cells sequentially. The notebook will:
* Mount your Google Drive.
* Install specific versions of required packages (e.g., `torch==2.0.1`, `mmcv==2.0.1`, `mmpose>=1.1.0`).
* Process the videos in the specified folder.
* Output processed CSVs (e.g., `{video_name}_alljt.csv`) and annotated visualization videos (e.g., `{video_name}_alljt.mp4`) back to your Google Drive.



*Note: You may be prompted to restart the runtime after the package installation cell. If so, restart and continue running the subsequent cells.*

## Limitations

* Validated on healthy adults; external validation in clinical populations with joint pathology is required.
* 2D pose estimation is sensitive to camera angle, occlusion, and parallax errors.
* While group-level analysis is highly reliable, individual clinical decisions should account for the reported SEM/MDC thresholds.

## Citation

If you use this repository or its methodology in your academic research, please cite:

```text
TBA

```

## License

MIT License
