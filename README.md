# nrh-train

**나란히SDK 보행 장애물 탐지 모델을 만드는 코드입니다.** AI Hub 「인도보행 영상」으로
YOLOv8n을 파인튜닝하고, 단말에서 돌릴 int8 ONNX까지 뽑습니다.

**[`pipeline.ipynb`](pipeline.ipynb) 하나에 다 들어 있습니다.** 위에서 아래로 읽으면
됩니다.

```
CVAT XML + 이미지                1. 데이터 준비    YOLO 형식으로 변환,
                                                  클래스 29종을 24종으로 병합
        ↓
images/ labels/ street.yaml      2. 학습           YOLOv8n 에서 100 epochs
        ↓
     best.pt   6.2 MB            3. ONNX 변환
        ↓
  float32.onnx  12.3 MB          4. int8 양자화
        ↓
    model.onnx   3.5 MB          5. 추론 확인
```

1단계가 만드는 것은 **Ultralytics 가 읽는 폴더 구조**입니다. 이미지 한 장에 라벨 텍스트
파일 하나가 짝을 이루고, `street.yaml` 이 학습과 검증 폴더의 위치와 클래스 번호를 적습니다.

```
labels/train/Bbox_0001/MP_SEL_000001.txt
  12 0.481250 0.627431 0.092188 0.351042
  클래스  중심x     중심y     너비     높이     ← 0~1 로 정규화
```

## 나오는 것

| | |
|---|---|
| 모델 | [J4VIS-2026/yolov8n-sidewalk-obstacle](https://huggingface.co/J4VIS-2026/yolov8n-sidewalk-obstacle) |
| 클래스 | 24종 — 볼라드, 가로수, 입간판, 사람, 차량 등 |
| 입력 | 640×640 RGB, 0~1 정규화, 레터박스 |
| 출력 | `[1, 28, 8400]` — 박스 4 + 클래스 점수 24. **NMS 미포함** |
| mAP50 | 0.6832 (검증셋 67,613장) |

## 돌린 환경

| | |
|---|---|
| GPU | NVIDIA RTX 5080 (16GB) — `batch=32` |
| CPU / RAM | AMD Ryzen 7 7800X3D / 48GB |
| OS | Windows 11 |
| Python | 3.14.2 |

노트북이 쓰는 패키지의 실제 판입니다.

```
--extra-index-url https://download.pytorch.org/whl/cu130

ultralytics==8.4.101
torch==2.13.0+cu130
onnx==1.22.0
onnxruntime==1.27.0
onnxslim==0.1.94
opencv-python==5.0.0.93
numpy==2.4.4
```

**`torch` 의 CUDA 판은 PyPI 기본 인덱스에 없습니다.** 첫 줄이 없으면
`torch==2.13.0+cu130` 을 못 찾습니다. `cu130` 은 CUDA 13.0 을 뜻합니다.

`onnxslim` 은 노트북이 직접 부르지는 않지만 3단계의 `simplify=True` 가 요구합니다.

**노트북의 경로는 이 기계 기준으로 적혀 있습니다.** `SOURCE_ROOT`, `OUTPUT_DIR`,
`PROJECT`, `IMAGE` 네 곳입니다.

## 학습 데이터를 여기에 두지 않는 이유

**AI Hub 이용 조건이 원본 이미지와 라벨의 재배포를 금지합니다.** 크기를 줄이거나 형식만
바꾼 것도 원본과 같게 취급됩니다. 그래서 이 저장소에는 **변환하는 코드만** 있습니다.

> 이 결과물은 AI 허브의 「인도보행 영상」 데이터셋을 활용하였습니다.

## 함께 보기

| | |
|---|---|
| 인식 모듈 | https://github.com/J4VIS-2026/nrh-detector |
| 통합 모듈 | https://github.com/J4VIS-2026/nrh |

## 라이선스

**AGPL-3.0-only** 입니다. Ultralytics YOLO 가 AGPL-3.0 이면서 "or later" 를 붙이지
않았으므로, 파생물인 이 저장소도 v3 로만 냅니다. 전문은 같은 폴더의 `LICENSE`, 가져다
쓴 것의 출처는 `NOTICE.md` 에 있습니다.

Copyright (C) 2026 J4VIS
