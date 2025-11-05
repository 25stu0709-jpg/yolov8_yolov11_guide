# yolov8-yolov11_guide

# 🧠 YOLOv11 정리 (for Beginners)

> **목표:** 초보자를 위한 YOLOv11 학습 및 정리 문서  
> 최신 YOLOv11을 실습하며 YOLOv8과의 차이점을 이해하고, YOLO의 핵심 개념을 정리합니다.

---

## 📘 YOLO 소개

**YOLO (You Only Look Once)** 는 이미지나 영상에서 객체를 **한 번에 탐지**하는 딥러닝 기반 객체 탐지(Object Detection) 모델입니다.  
속도와 정확도의 균형이 좋아, 자율주행, CCTV 분석, 로봇 비전 등 다양한 분야에서 사용됩니다.

---

## 🚀 YOLO 버전 비교

| 항목 | YOLOv8 | YOLOv11 |
|------|---------|----------|
| 출시 시기 | 2023년 | 2024년 (Ultralytics 발표) |
| 개발사 | Ultralytics | Ultralytics |
| 백본(Backbone) | CSPDarknet 변형 | ConvNeXt 기반으로 효율성 향상 |
| 속도 | 빠름 | 더 빠름 (최적화된 연산) |
| 정확도 (mAP) | 높음 | 약 3~5% 향상 |
| 지원 모델 | YOLOv8n, s, m, l, x | YOLOv11n, s, m, l, x |
| 학습 프레임워크 | PyTorch | PyTorch |
| 특징 | 실시간 탐지, 세그멘테이션, 추적 지원 | 향상된 학습 안정성, 경량화된 모델 |
| 설치 명령어 | `pip install ultralytics` | 동일 (`pip install ultralytics`) |
| 사용 명령어 | `yolo detect train ...` | 동일 (`yolo detect train ...`) |

---

## 🧩 YOLO 용어 정리

| 용어 | 설명 |
|------|------|
| **Bounding Box (BBox)** | 객체를 감싸는 사각형 영역 (x, y, width, height) |
| **Confidence Score** | 예측한 객체가 맞을 확률 (0~1 사이 값) |
| **Class** | 객체의 종류 (예: 사람, 자동차 등) |
| **mAP (mean Average Precision)** | 모델의 탐지 정확도를 평가하는 대표 지표 |
| **IoU (Intersection over Union)** | 예측 박스와 실제 박스의 겹침 비율 |
| **Anchor Box** | 다양한 비율·크기의 기준 박스 (YOLOv2~v5까지 사용) |
| **NMS (Non-Maximum Suppression)** | 중복된 탐지 박스를 제거하는 후처리 단계 |
| **Dataset** | 학습용 이미지 + 라벨 데이터 |
| **Label** | 각 이미지에 포함된 객체 위치 정보 (`.txt` 형식) |
| **Epoch** | 전체 데이터셋을 한 번 학습하는 주기 |
| **Batch Size** | 한 번에 학습하는 데이터 묶음의 크기 |
| **Loss Function** | 모델이 예측을 얼마나 잘못했는지를 계산하는 함수 |
| **Inference** | 학습된 모델을 이용해 실제 예측을 수행하는 과정 |

---

## ⚙️ YOLOv11 설치

```bash
# (선택) 가상환경 생성
python -m venv yolo_env
source yolo_env/bin/activate  # (Windows: yolo_env\Scripts\activate)

# Ultralytics 설치
pip install ultralytics

# 버전 확인
yolo version




# 🚀 YOLOv11 모델 성능 비교 및 다운로드

> **출처:** [Ultralytics 공식 문서](https://docs.ultralytics.com)

아래 표는 COCO 데이터셋으로 학습된 **YOLOv11 사전 학습(Pretrained) 모델**의 성능 비교표입니다.  
각 모델 이름(파란색)을 클릭하면 바로 모델을 다운로드할 수 있습니다.

---

## 🧠 감지 (Detection) 모델 성능 (COCO)

| 모델 | 크기 (픽셀) | mAP<sub>val</sub> 50–95 | 속도<br>(CPU ONNX, ms) | 속도<br>(T4 TensorRT10, ms) | 파라미터 (M) | FLOPs (B) | 다운로드 |
|:------|:-------------:|:----------------:|:------------------:|:--------------------:|:---------------:|:------------:|:-----------:|
| [YOLOv11n](https://github.com/ultralytics/assets/releases/download/v11.0/yolov11n.pt) | 640 | 39.5 | 56.1 ± 0.8 | 1.5 ± 0.0 | 2.6 | 6.5 | 🔽 [Download](https://github.com/ultralytics/assets/releases/download/v11.0/yolov11n.pt) |
| [YOLOv11s](https://github.com/ultralytics/assets/releases/download/v11.0/yolov11s.pt) | 640 | 47.0 | 90.0 ± 1.2 | 2.5 ± 0.0 | 9.4 | 21.5 | 🔽 [Download](https://github.com/ultralytics/assets/releases/download/v11.0/yolov11s.pt) |
| [YOLOv11m](https://github.com/ultralytics/assets/releases/download/v11.0/yolov11m.pt) | 640 | 51.5 | 183.2 ± 2.0 | 4.7 ± 0.1 | 20.1 | 68.0 | 🔽 [Download](https://github.com/ultralytics/assets/releases/download/v11.0/yolov11m.pt) |
| [YOLOv11l](https://github.com/ultralytics/assets/releases/download/v11.0/yolov11l.pt) | 640 | 53.4 | 238.6 ± 1.4 | 7.1 ± 0.2 | 35.3 | 86.9 | 🔽 [Download](https://github.com/ultralytics/assets/releases/download/v11.0/yolov11l.pt) |
| [YOLOv11x](https://github.com/ultralytics/assets/releases/download/v11.0/yolov11x.pt) | 640 | 54.7 | 462.8 ± 6.7 | 11.3 ± 0.2 | 56.9 | 194.9 | 🔽 [Download](https://github.com/ultralytics/assets/releases/download/v11.0/yolov11x.pt) |

---

## 📦 사용 예시

모델을 다운로드한 후, 아래와 같이 예측을 실행할 수 있습니다 👇

```bash
# 예측 실행
yolo detect predict model=yolov11s.pt source='image.jpg' show=True


