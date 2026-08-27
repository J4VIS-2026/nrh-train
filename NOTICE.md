# 남의 코드와 라이선스

**이 저장소는 AGPL-3.0-only입니다.** 전체 조문은 같은 폴더의 `LICENSE`에 있습니다.

**법률 자문이 아닙니다.** 사실만 적어둔 것이니, 공개 전에 판단이 필요하면 확인을 받으세요.

---

## 왜 AGPL인가 — 고를 수 있는 것이 아니었습니다

학습을 **Ultralytics YOLO로 했고 Ultralytics가 AGPL-3.0**입니다. AGPL은 그 코드를 쓴
전체 저작물도 같은 라이선스로 내놓게 합니다. **여기서 나온 가중치도 파생물입니다.**

---

## 무엇이 어디서 왔나

| 무엇 | 어디서 왔나 | 라이선스 |
|---|---|---|
| 학습·검증·ONNX 변환 | `ultralytics` 8.4.101 | **AGPL-3.0** |
| 학습 프레임워크 | `torch` 2.13.0 | BSD-3-Clause |
| ONNX 처리 | `onnx` 1.22.0, `onnxruntime` 1.27.0, `onnxslim` 0.1.94 | Apache-2.0 / MIT / MIT |
| 이미지 전처리 | `opencv-python` 5.0.0.93, `numpy` 2.4.4 | Apache-2.0 / BSD-3-Clause |
| `pipeline.ipynb` | 직접 짬 | 이 저장소와 같음 |

버전은 `requirements.txt`에 고정되어 있습니다.

---

## 학습 데이터

**AI Hub 「인도보행 영상」 데이터셋을 활용하였습니다.**

**원본 이미지와 라벨은 이 저장소에 없습니다.** 재배포가 금지되어 있어서입니다. 자르거나
박스를 그린 것도 원본으로 봅니다. 데이터를 두지 않는 이유는 `README.md`에 적어두었습니다.

가중치는 공개해도 된다는 답변을 받았습니다(2026-08-12). 결과물은
[huggingface.co/J4VIS-2026/yolov8n-sidewalk-obstacle](https://huggingface.co/J4VIS-2026/yolov8n-sidewalk-obstacle)에
있습니다.

---

Copyright (C) 2026 J4VIS
