# Segment Anything Model (SAM)

---

### 🧠 Segment Anything Model (SAM) 개요

**Meta AI**에서 개발한 SAM은 이미지나 비디오 속 객체를 자동으로 분할(Segmentation)할 수 있는 **범용 세분화 모델**입니다. 다양한 **입력 프롬프트**(클릭, 박스, 텍스트 등)를 바탕으로 원하는 객체의 **정확한 마스크**를 생성합니다.

---

### 🚩 주요 특징

* **프롬프트 기반 분할**: 클릭, 박스 등 입력에 따라 다양한 객체 분할 가능
* **제로샷(Zero-Shot) 수행**: 사전 학습 없이도 다양한 이미지에 일반화
* **SA-1B 데이터셋 학습**: 11억 개 마스크, 1,100만 장 이미지로 훈련됨

---

### ⚙️ 아키텍처 구성

1. **이미지 인코더**: 이미지 특징 추출
2. **프롬프트 인코더**: 사용자 입력 해석
3. **마스크 디코더**: 최종 분할 마스크 생성

---

### 🛠 활용 분야

* 의료 영상 분석 (세포, 병변 검출)
* 자율주행 (차량, 보행자 식별)
* 위성사진 분석 (건물, 도로 분할)
* 영상 편집 (객체 추출 및 제거)

---

### 🔗 오픈소스와 데모

* GitHub: [facebookresearch/segment-anything](https://github.com/facebookresearch/segment-anything)
* 데모: [segment-anything.com](https://segment-anything.com)
