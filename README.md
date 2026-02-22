---

# ROMAI

**Reliability and Validity of Artificial Intelligence-Based Pose Estimation in Measuring Joint Range of Motion**

ROMAI is an automated 2D human pose estimation and joint angle analysis pipeline built on **MMpose**. It extracts body keypoints from video and computes joint range of motion (ROM) automatically. The pipeline is designed for applications in sports science, rehabilitation, biomechanics research, and clinical assessment.

---

## Overview

This project provides a streamlined workflow for:

• 2D human pose estimation
• Automated joint angle calculation
• Batch video processing
• Google Colab-based execution (no complex local setup required)

The system is based on the OpenMMLab ecosystem, including MMpose, MMDetection, and MMCV.

---

## Key Features

### 1. 2D Human Pose Estimation

High-accuracy keypoint detection using state-of-the-art MMpose models.

### 2. Automatic Joint Angle Calculation

Joint angles are computed directly from detected keypoints using geometric vector calculations.

### 3. Batch Video Processing

Multiple video files can be processed sequentially with a single command.

### 4. Google Colab Support

Fully executable in Google Colab with GPU acceleration support.

---

## System Requirements

• Python ≥ 3.8
• CUDA ≥ 11.3 (GPU recommended)
• Google Colab environment (recommended configuration)

For GPU acceleration in Colab, ensure runtime is set to **GPU**.

---

## Installation (Google Colab Only)

Run the following in the first cell of your Colab notebook:

```python
# PyTorch (CUDA 11.8 build compatible with Colab)
!pip install torch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 --index-url https://download.pytorch.org/whl/cu118

# Clean conflicting packages
!pip uninstall -y jax jaxlib transformers accelerate torchao

# Core dependencies
!pip install numpy==1.24.4 pandas==1.5.3 scipy==1.10.1 requests==2.28.2 tqdm==4.65.2 filelock==3.14.0
!pip install transformers==4.30.2

# OpenMMLab installation
!pip install -U openmim
!mim uninstall mmengine mmcv mmdet mmpose -y
!mim install mmengine
!pip install mmcv==2.0.1 -f https://download.openmmlab.com/mmcv/dist/cu118/torch2.0/index.html
!mim install "mmdet<3.3.0"
!mim install "mmpose>=1.1.0"

# Clone repository
!git clone https://github.com/open-mmlab/mmpose.git
%cd mmpose
!pip install -r requirements.txt
!pip install -v -e .
%cd /content
```

After installation, **restart the Colab runtime** before proceeding.

---

## Usage

### 1. Prepare Video Files

Upload videos to the Colab working directory or mount Google Drive.

### 2. Run the Main Pipeline

Replace the script name if different:

```bash
python run_pose_estimation.py --input your_video.mp4 --output output_folder/
```

### 3. Output

The output directory will contain:

• Extracted keypoint data
• Computed joint angle results
• (Optional) Processed visualization video

---

## Output Structure Example

```
output_folder/
├── keypoints.json
├── joint_angles.csv
└── visualization.mp4
```

---

## Troubleshooting

• If dependency conflicts occur, rerun the installation block.
• Always restart the runtime after installation.
• Ensure GPU runtime is enabled in Colab.

---

## Applications

• Clinical range of motion assessment
• Rehabilitation outcome tracking
• Sports biomechanics analysis
• Movement research automation

---

## References

• MMpose Documentation: [https://mmpose.readthedocs.io](https://mmpose.readthedocs.io)
• OpenMMLab GitHub: [https://github.com/open-mmlab](https://github.com/open-mmlab)

---

## License

This project is licensed under the MIT License.

---
