# 🛠️ Realtime Assembly Guide
### Vision-based Smart Assembly Assistant

<div align="center">

![Python](https://img.shields.io/badge/Python_3.10-3776AB?style=for-the-badge&logo=python&logoColor=white)
![YOLO11](https://img.shields.io/badge/YOLO11-00FFFF?style=for-the-badge&logo=yolo&logoColor=black)
![PyQt5](https://img.shields.io/badge/PyQt5-41CD52?style=for-the-badge&logo=qt&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)

**비전 인식 기반 실시간 조립 가이드 시스템**

상부 고정 카메라와 YOLO 기반 AI 모델을 결합하여, 종이 설명서 없이 현재 조립 단계에 맞는 가이드를 실시간으로 제공하는 스마트 자동화 솔루션

[**▶ Demo Video on YouTube**](https://youtu.be/sAVkPxLrHNo)

<img width="800" alt="demo_video" src="https://github.com/user-attachments/assets/5067cc5d-ddf9-4f12-b74d-a83ca86ba3d0" />

</div>

---

## 📌 프로젝트 개요

본 프로젝트는 **컴퓨터 비전 + GUI 애플리케이션**을 결합한 AI 컨버전스 로봇 소프트웨어 프로젝트입니다.  
사용자는 조립 대상을 카메라 앞에 놓기만 하면, 시스템이 현재 단계를 자동으로 판별하고 다음 가이드를 제공합니다.

```
[카메라 입력] → [부품 검출] → [단계 판별] → [UI 가이드 표시] → [자동 단계 전환]
```

> DIY 조립, 3D 프린팅, 교육용 키트, 가구 조립 등 다양한 B2C 시나리오로 확장 가능

---

## 🏗️ 시스템 아키텍처

```
┌─────────────────────────────────────────────────────────┐
│                   Realtime Assembly Guide               │
│                                                         │
│  ┌──────────────┐                                       │
│  │  입력 계층    │  상부 고정 카메라 (Top-view)           │
│  └──────┬───────┘                                       │
│         │                                               │
│  ┌──────▼───────┐                                       │
│  │  전처리 계층  │  프레임 캡처 · 리사이즈 · 보정         │
│  └──────┬───────┘                                       │
│         │                                               │
│  ┌──────▼────────────────────────────────────────────┐  │
│  │                  인식 계층 (YOLO11)                │  │
│  │  ┌─────────────────┐    ┌────────────────────┐    │  │
│  │  │  Detect 모델     │    │   Classify 모델     │    │  │
│  │  │  (부품 존재/위치) │    │  (현재 조립 단계)   │    │  │
│  │  └─────────────────┘    └────────────────────┘    │  │
│  └──────┬────────────────────────────────────────────┘  │
│         │                                               │
│  ┌──────▼───────┐                                       │
│  │  판단 계층    │  단계 게이팅 · 신뢰도 조건 · 체크리스트 │
│  └──────┬───────┘                                       │
│         │                                               │
│  ┌──────▼────────────────────────────────────────────┐  │
│  │  안내 계층 (PyQt5 UI)                              │  │
│  │  텍스트 가이드 · 이미지 · 부품 체크리스트 표시      │  │
│  └──────┬────────────────────────────────────────────┘  │
│         │                                               │
│  ┌──────▼───────┐                                       │
│  │  콘텐츠 계층  │  모델 파일 · 단계별 텍스트 · 이미지   │
│  └──────────────┘                                       │
└─────────────────────────────────────────────────────────┘
```

---

## ✨ 핵심 기능

| 기능 | 설명 |
|------|------|
| 🎥 실시간 부품 검출 | YOLO Detect로 카메라 프레임 내 부품 존재 여부 및 위치 파악 |
| 🔢 조립 단계 판별 | YOLO Classify로 현재 조립 단계 자동 분류 |
| ⚡ 자동 가이드 전환 | 단계 조건 충족 시 다음 가이드로 자동 이동 |
| ✅ 부품 체크리스트 | 누락 부품을 UI에서 실시간 확인 |
| 🛡️ 게이팅 로직 | 잘못된 자동 진행 방지를 위한 신뢰도 기반 필터링 |
| 💻 로컬 독립 실행 | 외부 서버 없이 로컬 환경에서 완전 동작 |

---

## 🎯 대표 PoC: Foosball 3D 프린팅 조립

본 프로젝트의 핵심 검증 대상은 **Foosball 3D 프린팅 조립물**입니다.

| 항목 | 내용 |
|------|------|
| 부품 종류 | 14종 |
| 총 부품 수 | 30개 |
| 표준 조립 단계 | 8단계 |
| 학습용 확장 단계 | 17단계 |

추가로 **Raspberry Pi 5 케이스** 및 **PCB 받침대 가이드**에도 확장 적용되었습니다.

---

## 📈 모델 학습 고도화 전략 (Progressive Training)

현장 수준의 견고한 AI 모델을 구축하기 위해 **점진적 데이터 학습 파이프라인**을 설계했습니다.

**STEP 1. 단일 부품 데이터 구축**  
각 부품의 특징을 독립적으로 파악하기 위한 기초 데이터 수집  
<img width="600" alt="그림1" src="https://github.com/user-attachments/assets/df742431-25a0-4264-ba6b-e4c65d647174" />

**STEP 2. 단일 학습**  
개별 부품에 대한 베이스라인 Detection 모델 훈련  
<img width="600" alt="그림2" src="https://github.com/user-attachments/assets/c1b6ea1a-cc6d-47e0-8efd-8588d4a8ab0d" />

**STEP 3. 복합 환경 구성**  
부품 겹침, 손 가림 등 실제 조립 환경의 복잡한 Workspace 모사  
<img width="600" alt="그림3" src="https://github.com/user-attachments/assets/bec4a407-452a-42db-800e-707615aff508" />

**STEP 4. 복합 학습 (Fine-tuning)**  
복합 환경 데이터를 추가 투입하여 실전 대응력 강화  
<img width="600" alt="그림4" src="https://github.com/user-attachments/assets/52d7928d-263a-45f5-bc6a-4ad6eeb06be2" />

---

## 📊 모델 성능 (Foosball 기준)

### Detect 모델 (YOLO11n)

| 지표 | 수치 |
|------|------|
| mAP@50 | **97.85%** |
| mAP@50-95 | **84.51%** |

### Classify 모델 (YOLO11n-cls)

| 지표 | 수치 |
|------|------|
| Top-1 Accuracy | **95.37%** _(3개 모델 평균)_ |

> 위 수치는 Foosball 중심 데이터셋 및 실험 환경 기준입니다.

---

## 🔧 트러블슈팅 및 최적화

**문제**  
단일 부품 위주로 학습된 초기 모델이 복합 조립 환경(부품 겹침, 가림)에서 Confidence Score 저하 및 오탐지 발생

**해결**

1. 빈 작업 환경을 `Default` 클래스로 별도 학습 → 오탐지 방지
2. 미완성/완성 상태 이미지를 직접 라벨링 및 전처리 → 데이터셋 품질 향상
3. 프레임 스킵 로직 적용 → 실시간 추론 연산 부하 최소화

**결과**  
실제 조립 환경에서의 객체 인식 안정성을 대폭 확보. AI 모델의 한계를 데이터 품질 개선으로 극복

---

## 🔨 기술 스택

| 분류 | 기술 |
|------|------|
| Language | Python 3.10 |
| Vision AI | Ultralytics YOLO11n / YOLO11n-cls |
| GUI | PyQt5 |
| Image Processing | OpenCV |
| Deep Learning | PyTorch, Torchvision |
| Labeling | labelImg |
| Environment | Windows 11 |

---

## 🚀 설치 및 실행

```bash
# 1. 레포지토리 클론
git clone https://github.com/Tran9523/Realtime_Assembly_Guide.git
cd Realtime_Assembly_Guide

# 2. UI 애플리케이션 실행
cd app
python Realtime_Assembly_Guide.py

# 3-1. Detect 모델 샘플 학습
cd ../train
python prepare_dataset.py
python train_yolo_cls.py

# 3-2. Classify 모델 샘플 학습
python train_yolo_all.py
```

---

## 🗂️ 프로젝트 구조

```
Realtime_Assembly_Guide/
├── README.md
├── app/
│   ├── Realtime_Assembly_Guide.py   # 메인 애플리케이션
│   ├── assets/
│   │   ├── foosball/                # Foosball 가이드 리소스
│   │   ├── plates_tool/             # PCB 받침대 리소스
│   │   └── raspberry_pi/           # Raspberry Pi 케이스 리소스
│   └── YOLO11_model/
│       ├── detect_all/weights/best.pt
│       ├── classify_foosball/weights/best.pt
│       ├── classify_plates/weights/best.pt
│       └── classify_raspberry/weights/best.pt
└── train/
    ├── dataset_detect_parts/
    │   ├── images_all/
    │   └── labels_all/
    ├── source/
    │   ├── step0/
    │   ├── step1/
    │   └── step1_error/ ...
    ├── prepare_dataset.py
    ├── train_yolo_all.py
    └── train_yolo_cls.py
```

---

## 🔭 향후 확장 방향

- [ ] 엣지 디바이스 배포 (Jetson Nano / Raspberry Pi)
- [ ] 서버-클라이언트 분리 구조 전환
- [ ] 신규 조립 대상 추가 (범용 가이드 프레임워크화)
- [ ] 음성 안내 기능 연동

---

*AI Convergence Robotics Software Vision Project*
