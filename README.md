# romai
Reliability and Validity of Artificial Intelligence-Based Pose Estimation in Measuring Joint Range of Motion

MMpose-based 2D Pose Estimation & Joint Angle Analysis Pipeline
Overview
This project provides an automated pipeline for 2D human pose estimation and joint angle analysis using MMpose.
It enables easy pose extraction from videos and automated calculation of joint angles for sports, rehabilitation, and research purposes.

Features
2D Human Pose Estimation: Accurate keypoint detection using state-of-the-art MMpose models.

Joint Angle Calculation: Automated measurement of joint angles from detected keypoints.

Batch Video Processing: Supports multiple video files for large-scale analysis.

Google Colab & Local Support: Easy to use in both Colab and local (Ubuntu) environments.

Requirements
Python 3.8+

CUDA 11.3+ (for GPU acceleration)

See Requirements for detailed package and installation instructions.

Installation
Google Colab (Recommended)
Copy and run the following in the first cell of your Colab notebook:

python
# Install requirements
!pip install torch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 --index-url https://download.pytorch.org/whl/cu118
!pip uninstall -y jax jaxlib
!pip install numpy==1.24.4 pandas==1.5.3 scipy==1.10.1 requests==2.28.2 tqdm==4.65.2 filelock==3.14.0
!pip uninstall -y transformers accelerate torchao
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
After installation, restart the Colab runtime.

Local (Ubuntu)
Create and activate a conda environment:

bash
conda create -n openmmlab python=3.8 -y
conda activate openmmlab
Install dependencies (see requirements.md for full details).

Usage
Prepare your videos: Place video files in your working directory or Google Drive.

Run the main pipeline script (replace with your actual script name):

python
python run_pose_estimation.py --input your_video.mp4 --output output_folder/
Results: Keypoint data and joint angle analysis will be saved in the specified output folder.

Troubleshooting
If you encounter errors due to package version mismatches or missing installations, please rerun the installation script above.

In Colab, make sure to restart the runtime after installation to ensure proper operation.

References
MMpose Documentation

OpenMMLab GitHub

License
This project is licensed under the MIT License.


If you encounter errors due to package version mismatches or missing installations, please rerun the installation script above.
패키지 버전 불일치, 미설치 오류 발생 시 위 설치 스크립트를 다시 실행하세요.

In Colab, make sure to restart the runtime after installation to ensure proper operation.
Colab에서 "런타임 재시작" 후 다시 실행해야 정상 동작합니다.
