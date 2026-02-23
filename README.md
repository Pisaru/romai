ChatGPT가 맥락을 제대로 파악하지 못하고 엉뚱한 결과물을 내놓아서 많이 답답하셨겠네요. 물리치료사로서의 임상적 전문성과 AI 엔지니어링 역량이 모두 깊게 담긴 훌륭한 연구인데, 그 가치가 README에 정확히 반영되지 않으면 안 되죠.

제공해주신 논문 초록과 전체 내용, 그리고 실제 구동되는 Colab 기반의 코드(`MMpose_Rt_Shld.ipynb`)를 면밀히 분석했습니다.

요청하신 대로 `bland_altman.png`가 화면에 렌더링되지 않도록 이미지를 호출하는 코드는 삭제했고, 기존 초안에 있던 존재하지 않는 파이썬 스크립트 실행 명령어(`python run_pose_estimation.py`) 대신 실제 환경에 맞게 **Colab 노트북 기반의 사용법**으로 전면 수정했습니다. 또한 논문에 기재된 정확한 수치와 구조를 반영하여, 학술적 신뢰도를 높이면서도 오픈소스 사용자들이 직관적으로 이해할 수 있도록 다듬었습니다.

그대로 복사해서 `README.md`에 붙여넣으실 수 있도록 영문으로 작성된 최종본을 제공해 드립니다.

---

```markdown
# ROMAI

[cite_start]**Reliability and Validity of Artificial Intelligence–Based Pose Estimation in Measuring Joint Range of Motion** [cite: 1]

[cite_start]ROMAI is a research-based automated 2D human pose estimation and joint angle analysis pipeline built on OpenMMLab’s MMpose framework (v1.3.2) utilizing the HRNet-W32 architecture[cite: 12]. [cite_start]It extracts human keypoints from standard 2D video and automatically computes joint range of motion (ROM) without the need for markers or physical sensors[cite: 5, 94]. 

[cite_start]This repository accompanies a clinical validation study evaluating the intra- and inter-instrument reliability and concurrent validity of this AI-based approach against the universal goniometer (clinical gold standard) and a commercial IMU-based motion capture system (Noraxon myoMOTION)[cite: 6, 11].

## Project Overview

ROMAI provides a fully automated pipeline designed for Google Colab:
* [cite_start]2D markerless human pose estimation using pre-trained HRNet-W32 [cite: 94]
* [cite_start]Automated joint angle computation using standard trigonometric calculations from extracted keypoints (e.g., shoulder, elbow, hip) [cite: 95, 96]
* Batch video processing that outputs both raw CSV keypoint data and angle-annotated visualization videos
* [cite_start]Clinically validated measurement performance for shoulder flexion, extension, and abduction [cite: 6, 180]

**Target applications:**
* [cite_start]Clinical ROM assessment and rehabilitation outcome monitoring [cite: 181]
* [cite_start]Telehealth and remote patient monitoring [cite: 47]
* Sports biomechanics and research reproducibility

## Demo

![ROMAI Demo](docs/demo.gif)

*(A short demonstration of the automated joint tracking and angle overlay during shoulder movement.)*

## Validation Results

[cite_start]The validation summaries below reflect the core results from our comparative study[cite: 159]. 

### Sample & Protocol
* [cite_start]**Sample:** 40 healthy adults (23 males, 17 females) [cite: 9]
* **Protocol:** Three distinct shoulder movements (flexion, extension, abduction). [cite_start]Each movement was performed three times consecutively with a 3-second hold at the terminal range[cite: 105, 106, 108].
* [cite_start]**Setup:** Simultaneous concurrent measurement utilizing a universal goniometer, IMU sensors (100 Hz), and ROMAI (1080p, 60fps video)[cite: 11, 86, 91, 109].

### Reliability & Agreement Summary
* [cite_start]**Within-Instrument Reliability (Pose Estimation):** Demonstrated excellent intra-session reliability with ICC₂,₃ values of .968 (flexion), .984 (extension), and .976 (abduction)[cite: 139]. [cite_start]SEM ranged from 0.73–1.05°[cite: 139].
* [cite_start]**Inter-Method Reliability (vs. Goniometer):** Showed good to excellent agreement with the goniometer (ICC₂,₁ = .812–.939), which was superior to the agreement between the goniometer and IMU (ICC₂,₁ = .343–.873)[cite: 143, 144].
* [cite_start]**Bland–Altman Agreement:** Mean differences (bias) between the goniometer and pose estimation ranged narrowly from −1.03° to 3.32°[cite: 14]. 
* [cite_start]**Regression Analysis:** AI-based pose estimation uniquely and significantly predicted goniometric ROM across all movements (p < .001, Standardized β = .362–.620), whereas the IMU system failed to achieve statistical significance[cite: 15, 154].

*Conclusion: AI-based 2D pose estimation (ROMAI) demonstrated excellent repeatability and acceptable agreement with the universal goniometer, outperforming the tested commercial IMU system. [cite_start]It serves as a viable, contact-free alternative for clinical ROM assessment[cite: 167, 180].*

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

ROMAI is optimized to run in a Google Colab environment utilizing a GPU runtime. All execution logic is contained within the `MMpose_Rt_Shld.ipynb` notebook.

1. **Open the Notebook:** Upload `MMpose_Rt_Shld.ipynb` to your Google Drive and open it with Google Colab.
2. **Set Runtime:** Ensure your Colab runtime is set to GPU (T4 or higher is recommended).
3. **Configure Path:** In the first cell, update the `MY_PATH` variable to point to the folder in your Google Drive where your input videos (`.mp4`, `.mov`) are stored.
```python
MY_PATH = "sample_img/action_video"  # Update this to your specific Drive folder

```


4. **Run Cells:** Execute the cells sequentially. The notebook will automatically:
* Mount your Google Drive
* Install specific required package versions (e.g., `torch==2.0.1`, `mmcv==2.0.1`, `mmpose>=1.1.0`)
* Clone the `mmpose` repository and build it from source
* Run the `MMPoseInferencer` on your videos
* Calculate specific angles (e.g., Right Shoulder) and output annotated videos (`done_vis/`) and CSV data (`done_csv/`)
* Copy the final processed files back to your Google Drive folder



*Note: Following the package installation cell, the script may prompt you to **Restart Runtime**. Please restart the runtime and execute the remaining cells to ensure proper library linking.*

## Limitations

* The current validation sample is limited to healthy adults without major orthopedic or neurological histories. Generalizability to clinical populations with severe joint pathology requires independent validation.


* As a 2D pose estimation approach, the system is potentially susceptible to parallax error if the camera angle deviates significantly from the perpendicular to the movement plane.



## Citation

If you use this repository or its methodology in your academic research, please cite the accompanying validation study:

```bibtex
@article{yoo2026romai,
  title={Reliability and Validity of AI-Based Pose Estimation for Measuring Shoulder Range of Motion},
  author={Yoo, Seung-hun and [Co-authors]},
  journal={[Journal Name]},
  year={2026},
  doi={[DOI when available]}
}

```

## License

This project is licensed under the MIT License - see the [LICENSE](https://www.google.com/search?q=LICENSE) file for details.

```

---

**수정 및 반영된 주요 포인트:**
1. **정확한 논문 인용**: docx 파일에서 제시하신 ICC 수치, 참가자 수(40명), Bland-Altman Bias 값 등을 정확히 추출하여 반영했습니다.
2. **코드 구동 방식 현실화**: `python run_pose_estimation.py` 같은 가상의 코드를 지우고, 보내주신 `MMpose_Rt_Shld.ipynb` 환경(구글 드라이브 마운트, `MY_PATH` 설정, 런타임 재시작 등)에 맞는 정확한 사용법을 기재했습니다.
3. **요청사항 엄수**: `bland_altman.png`는 Repository Structure 구조도에만 남겨두고, 본문 내 렌더링(이미지 태그)은 제거했습니다.

혹시 리포지토리에 포함할 `requirements.md` (또는 `txt`) 파일도 위 노트북 코드 기반으로 깔끔하게 정리해 드릴까요? 필요하신 부분이 있다면 언제든 말씀해 주십시오.

```
