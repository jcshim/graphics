# MedSAM2

의료 영상 분할(Medical Image Segmentation)을 위한 모델들로, Meta의 Segment Anything Model(SAM)을 기반으로 **의료 분야에 특화**되도록 조정된 버전들입니다. 두 모델의 주요 차이점은 다음과 같습니다:

---

### 🔬 1. **기반 아키텍처 및 발전 과정**

| 항목     | MedSAM                        | MedSAM2               |
| ------ | ----------------------------- | --------------------- |
| 기반 모델  | Segment Anything Model (SAM)  | SAM에서 발전된 모델 (개선된 구조) |
| 릴리즈 시기 | 2023년                         | 2024년 중반 이후           |
| 목적     | 의료 이미지(CT, MRI 등) 분할 최적화      | 더 빠르고 정확하며 가볍게 개선된 모델 |
| 주요 개선점 | SAM의 encoder + 의료 fine-tuning | 새로운 아키텍처 + 효율성 개선     |

---

### ⚙️ 2. **기술적 차이**

| 비교 항목       | MedSAM                                         | MedSAM2                                |
| ----------- | ---------------------------------------------- | -------------------------------------- |
| **프롬프트 입력** | Point, Box 등 SAM 방식 유지                         | Point 외에도 다양한 sparse guidance 활용 가능    |
| **속도**      | SAM 기반이라 무거움 (ViT 기반, 느린 추론)                   | 경량화된 구조 → 빠른 추론 가능                     |
| **정확도**     | SAM 대비 높은 의료 이미지 성능                            | MedSAM보다 더 높은 Dice Score 보고됨           |
| **추론 구조**   | 이미지 → encoder → decoder + mask                 | 더 단순한 decoder 및 multi-scale 활용 구조      |
| **사용 데이터**  | Medical SAM dataset (기존 public data fine-tune) | 확장된 데이터셋 + more diverse medical images |

---

### 🧪 3. **실험 및 성능**

* **MedSAM**: SAM 모델에 비의료 이미지를 제거하고 CT/MRI용 fine-tuning만 수행 → 일반 SAM보다 훨씬 나은 성능.
* **MedSAM2**: MedSAM의 구조적 병목 개선 + 의료 영상에 맞춘 데이터 증강 및 mask refinement 기술 활용 → **성능 & 속도 동시 향상**.

---

### 📁 4. **공식 리소스**

| 항목     | MedSAM                                                                       | MedSAM2                                                                        |
| ------ | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| GitHub | [https://github.com/bowang-lab/MedSAM](https://github.com/bowang-lab/MedSAM) | [https://github.com/bowang-lab/MedSAM2](https://github.com/bowang-lab/MedSAM2) |
| 데모     | [MedSAM HuggingFace Space](https://huggingface.co/spaces/bowang/MedSAM)      | 향후 배포 예정 또는 연구 중                                                               |

---

### ✅ 요약

| 항목    | MedSAM               | MedSAM2            |
| ----- | -------------------- | ------------------ |
| 핵심 특징 | SAM을 의료용으로 fine-tune | 경량화 + 정확도 향상 구조    |
| 속도    | 느림                   | 빠름                 |
| 정확도   | SAM보다 좋음             | MedSAM보다 더 좋음      |
| 목적    | 의료 영상 분할             | 실시간 의료 영상 분석에 더 적합 |

---

필요하시면 논문 비교, 모델 구조 그림, 또는 실습 코드(PyTorch/HuggingFace)도 제공해 드릴 수 있습니다.
