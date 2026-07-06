# YOLO-example

Google Colab 환경에서 YOLO 객체 탐지 모델을 학습하고 실제로 활용하는 예제들을 모아둔 저장소입니다.

## 프로젝트 개요

이 프로젝트는 KITTI 데이터셋을 활용하여 자동차, 트럭, 밴을 탐지하는 YOLOv12 모델을 학습하고, 실시간 영상 분석, Vision Language Model(VLM) 통합 등 다양한 응용 방식을 다룹니다.

## Repository Status

This is a computer-vision learning/example repository, not a benchmark repository.

Current evidence:

- Colab-style scripts for KITTI conversion/training, webcam inference, segmentation, VLM analysis, and YouTube inference are included.
- Sample images and reference PDFs are included.
- Trained weights, mAP metrics, FPS benchmarks, and curated demo videos are not included.

Missing portfolio evidence:

- Training metrics: TBD
- mAP / precision / recall table: TBD
- Example annotated output image or GIF: TBD
- Reproducible Colab notebook links: TBD

## 주요 파일 설명

### 1. KITTI_data_YOLO
**KITTI 데이터셋으로 YOLOv12 모델 학습**

Google Colab 환경에서 실행되는 6개 셀로 구성됩니다:

- **셀 1: 라벨 변환** - KITTI 포맷(좌상단x1, y1, 우하단x2, y2)을 YOLO 포맷(중심x, y, 너비, 높이)으로 변환
  - 클래스 매핑: Car(0), Truck(1), Van(2)
  
- **셀 2: 데이터 분할** - 전체 이미지를 train/val 80:20 비율로 분할
  
- **셀 3: 설정 파일 생성** - YOLO 학습에 필요한 yaml 설정 (경로, 클래스 수, 이름)
  
- **셀 4: 모델 학습** - YOLOv12n으로 50 에포크 학습 (batch size 16, 이미지 640x640)
  
- **셀 5: 검증 이미지** - 학습된 모델로 검증 셋 이미지 1장 테스트
  
- **셀 6: 추론** - 사용자 이미지에서 자동차 탐지 및 저장

**사용 기술:** Ultralytics YOLO12, PIL, scikit-learn

---

### 2. KITTI_YOLO_seg
**YOLOv11 세그멘테이션으로 객체 경계선 예측**

단순하지만 강력한 세그멘테이션 예제입니다:

- YOLOv11n-seg 모델 사용 (nano 버전, 빠른 속도)
- 사용자 이미지 디렉토리에 대해 추론 실행
- 신뢰도 임계값 0.25 이상의 객체만 탐지
- 결과를 이미지 파일로 저장 (마스크와 경계선 표시)

**특징:** 객체 탐지뿐 아니라 정확한 픽셀 레벨 경계선을 제공합니다.

---

### 3. YOLO_real_time_detection
**웹캠 실시간 객체 탐지 (Google Colab)**

Google Colab에서 웹캠 접근을 활용한 실시간 탐지입니다:

- **UI 구성:** 640x480 해상도로 좌측에 원본 영상, 우측에 탐지 결과 표시
- **처리:** JavaScript로 웹캠 스트림 캡처 → Base64 인코딩 → Python YOLO 처리 → 결과 출력
- **감지:** 신뢰도 0.3 이상의 모든 객체 탐지
- **중지 버튼:** "중지" 버튼으로 언제든 종료 가능

**처리 흐름:**
```
웹캠 프레임 → Base64 → YOLO 추론 → 바운딩박스 그리기 → 화면에 표시 (반복)
```

---

### 4. VLM (Vision Language Model)
**OpenAI GPT-4와 결합한 고급 이미지 분석**

웹캠 영상을 GPT-4에 전달하여 자연어로 분석합니다:

- **설정:** `OPENAI_API_KEY`를 Colab Secrets 또는 환경 변수로 설정
- **분석 모드:**
  - `caption`: 장면 전체를 한 문장으로 설명
  - `objects`: 보이는 주요 물체 5개 이하로 나열
  - `action`: 사람의 행동 설명
  - `ocr`: 이미지 속 글자 인식 및 요약
  - `custom_1`, `custom_2`: 사용자 정의 프롬프트

- **실행 파라미터:**
  - `mode`: 분석 모드 선택
  - `analysis_interval`: 분석 간격 (기본 4초)
  - `display_prefix`: 화면 표시 제목

- **UI:** YOLO_real_time_detection과 동일하게 좌측 원본, 우측 분석 결과 표시

**장점:** 단순 객체 탐지를 넘어 **장면 이해**와 **의미 분석**이 가능합니다.

---

### 5. youtube_car_detection
**YouTube 영상 분석**

YouTube 링크로부터 직접 자동차 탐지를 수행합니다:

- **라이브러리:** yt-dlp로 YouTube 스트림 다운로드
- **모델:** 학습된 KITTI 모델 또는 사전학습 모델 사용 가능
- **처리:**
  - 신뢰도 0.25 이상만 탐지
  - 스트리밍 모드로 메모리 효율적 처리
  - 결과를 MP4 동영상으로 저장
  
- **처리량:** 첫 300프레임만 처리 (시간 제한)

**용도:** 실제 도로 영상이나 자동차 관련 동영상에서 자동차 탐지 성능 검증

---

## 파일 구조

```
YOLO-example/
├── README.md                          # 이 파일
├── .gitignore                         # Git 무시 목록
├── LICENSE                            # MIT 라이센스
├── YOLO.pdf                           # YOLO 논문
├── YOLO12_example.ipynb - Colab.pdf  # Colab 예제 스크린샷
├── key.pdf                            # 참고 자료
├── KITTI_data_YOLO                    # 셀별 학습 스크립트
├── KITTI_YOLO_seg                     # 세그멘테이션 예제
├── YOLO_real_time_detection           # 웹캠 실시간 탐지
├── VLM                                # GPT-4 통합 분석
├── youtube_car_detection              # YouTube 영상 분석
└── car_Test/                          # 테스트용 자동차 이미지 디렉토리
```

## 필수 라이브러리

```bash
pip install ultralytics opencv-python pillow yt-dlp openai scikit-learn
```

## Secret Handling

Do not hard-code API keys in this repository.

For Colab VLM examples, store the key as a Colab Secret named `OPENAI_API_KEY`, or set an environment variable before running the script.

```python
import os
from google.colab import userdata

OPENAI_API_KEY = userdata.get("OPENAI_API_KEY") or os.environ.get("OPENAI_API_KEY")
```

## 사용 방법

### 1. 기본 YOLO 학습 (Google Colab 권장)
```
1. Google Colab 노트북 생성
2. KITTI 데이터셋 다운로드 및 업로드
3. KITTI_data_YOLO 스크립트 셀 순서대로 실행
```

### 2. 실시간 탐지
```
1. Colab에서 YOLO_real_time_detection 실행
2. 웹캠 접근 권한 허락
3. 좌측 원본 / 우측 탐지 결과 확인
```

### 3. VLM 분석
```
1. OpenAI API 키를 VLM 파일에 입력
2. 분석 모드 선택 (caption, objects, action, ocr 등)
3. Colab에서 실행 후 자연어 분석 결과 확인
```

### 4. YouTube 분석
```
1. youtube_car_detection에서 YouTube 링크 수정
2. 학습된 모델 경로 지정
3. 실행 후 결과 동영상 다운로드
```

## 라이센스

MIT License - 자유롭게 사용, 수정, 배포할 수 있습니다.

## 참고

- **YOLO 공식:** https://github.com/ultralytics/ultralytics
- **KITTI 데이터셋:** http://www.cvlibs.net/datasets/kitti/
- **Google Colab:** https://colab.research.google.com/
