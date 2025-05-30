# romai
Reliability and Validity of Artificial Intelligence-Based Pose Estimation in Measuring Joint Range of Motion

Automated 2D Pose Estimation & Joint Angle Analysis Pipeline (MMpose-based)
Overview
This project provides an automated pipeline for 2D human pose estimation and joint angle analysis using MMpose.
You can easily extract poses from videos and automatically calculate joint angles, making it useful for sports, rehabilitation, research, and more.

Features
2D Human Pose Estimation: Accurate keypoint detection using state-of-the-art MMpose models

Automatic Joint Angle Calculation: Computes key joint angles from detected keypoints

Batch Video Processing: Analyze multiple video files at once

Google Colab Support: No complex setup—run directly in Colab

Requirements
Python 3.8 or higher

CUDA 11.3 or higher (for GPU acceleration)

For detailed package and installation instructions, see Requirements.

Installation (Google Colab Only)
Copy and run the following in the first cell of your Colab notebook:

python
# Install required packages
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

Usage
Prepare your videos: Place video files in your Colab working directory or Google Drive.

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

MMpose 기반 2D 포즈 추정 및 관절 각도 분석 자동화 파이프라인
개요
이 프로젝트는 MMpose를 활용한 2D 인체 포즈 추정 및 관절 각도 자동 분석 파이프라인입니다.
비디오에서 손쉽게 포즈를 추출하고, 관절 각도를 자동으로 계산할 수 있어 스포츠, 재활, 연구 등 다양한 분야에 활용할 수 있습니다.

주요 기능
2D 인체 포즈 추정: 최신 MMpose 모델을 활용한 정확한 관절 키포인트 검출

관절 각도 자동 계산: 검출된 키포인트로부터 주요 관절 각도를 자동 산출

배치 비디오 처리: 여러 개의 비디오 파일을 한 번에 분석 가능

Google Colab 지원: 별도의 환경 설정 없이 Colab에서 바로 사용 가능

요구사항
Python 3.8 이상

CUDA 11.3 이상 (GPU 사용 시)

자세한 패키지 및 설치 방법은 Requirements 문서를 참고하세요.

설치 방법 (Google Colab 전용)
Colab 노트북의 첫 번째 셀에 아래 코드를 복사해서 실행하세요.

python
# 필수 패키지 설치
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
설치가 완료되면 Colab 런타임을 재시작하세요.

사용법
분석할 비디오 준비: Google Drive 또는 Colab 작업 디렉토리에 비디오 파일을 준비합니다.

메인 파이프라인 스크립트 실행 (실제 스크립트명으로 변경):

python
python run_pose_estimation.py --input your_video.mp4 --output output_folder/
결과 확인: 지정한 output 폴더에 키포인트 데이터와 관절 각도 분석 결과가 저장됩니다.

트러블슈팅
패키지 버전 불일치, 미설치 오류 발생 시 위 설치 스크립트를 다시 실행하세요.

Colab에서는 설치 후 반드시 런타임을 재시작해야 정상 동작합니다.

참고 자료
MMpose 공식 문서

OpenMMLab GitHub

라이선스
본 프로젝트는 MIT 라이선스를 따릅니다.
