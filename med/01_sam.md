# Segment Anything Model(SAM)

---

## 🔍 핵심 개념 요약

| 항목        | 설명                                |
| --------- | --------------------------------- |
| **모델명**   | Segment Anything Model (SAM)      |
| **개발기관**  | Meta AI                           |
| **출시 시기** | 2023년 4월 공개                       |
| **주요 목표** | 어떤 객체든 사용자의 간단한 입력에 따라 정확하게 분할    |
| **입력 방식** | 점(Point), 박스(Box), 텍스트 등 다양한 프롬프트 |
| **출력**    | 마스크 이미지 (해당 객체의 픽셀 영역)            |

---

## 🧠 SAM의 주요 특징

### 1. **Promptable Segmentation**

* SAM은 **사용자 프롬프트**에 따라 작동합니다.
* 프롬프트의 종류:

  * **점 클릭(positive/negative point)**: 여기를 포함해줘/제외해줘
  * **바운딩 박스**: 이 안의 물체를 분할해줘
  * **텍스트(다른 모델과 결합 시)**: "고양이" 영역을 분할해줘

### 2. **Zero-shot 성능**

* SAM은 사전에 학습된 거대한 데이터셋 덕분에 **새로운 이미지나 객체에 대해 별도의 학습 없이도 분할 가능**합니다.
* 즉, **Zero-shot segmentation**이 가능해 다양한 도메인에 바로 적용할 수 있습니다.

### 3. **대규모 학습 데이터**

* SAM은 \*\*SA-1B(Segment Anything 1 Billion)\*\*이라는 **11억 개의 마스크가 포함된 세계 최대 분할 데이터셋**으로 학습되었습니다.
* 1천만 개 이상의 이미지와 다양한 객체를 포함하고 있음.

---

## 🧩 SAM의 구성

SAM은 크게 **세 개의 컴포넌트**로 구성됩니다:

1. **Image Encoder (ViT 기반)**

   * 이미지를 고차원 임베딩으로 변환
2. **Prompt Encoder**

   * 사용자의 입력(점, 박스 등)을 임베딩으로 변환
3. **Mask Decoder**

   * 이미지와 프롬프트 임베딩을 결합해 **분할 마스크** 생성

---

## 🖼️ 활용 예시

* 사진 편집: 객체 추출 및 배경 제거
* 의료 영상 분석: 병변 자동 분할
* 자율 주행: 도로 객체 인식
* 로보틱스: 물체 탐지 및 조작
* AR/VR: 실시간 객체 트래킹 및 환경 이해

---

## 🔧 사용 방법

* Meta에서 **Python용 API와 Web Demo**를 제공하고 있음
* PyTorch 기반 모델로 사용 가능하며, HuggingFace에서도 배포됨
* GitHub: [https://github.com/facebookresearch/segment-anything](https://github.com/facebookresearch/segment-anything)

---

## 📌 요약

> Segment Anything Model은 “**어떤 객체든, 어떤 입력이든, 어떤 이미지든**” 분할 가능한 범용 이미지 분할 모델로, 다양한 AI 응용의 기초가 될 수 있는 **Foundation Vision Model**입니다.
