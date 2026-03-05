# ml5.js는 JavaScript 기반의 오픈소스 머신러닝 라이브러리

---

### ✅ 주요 특징

* **TensorFlow\.js 기반**: ml5.js는 TensorFlow\.js 위에 구축되어, 복잡한 설정 없이 브라우저에서 머신러닝 기능을 사용할 수 있습니다.([CSDN][1])

* **친숙한 API**: p5.js와 유사한 방식으로 설계된 API를 통해, 몇 줄의 코드만으로 이미지 분류, 포즈 추정, 스타일 전이 등 다양한 기능을 구현할 수 있습니다.

* **다양한 사전 학습 모델 지원**:

  * 이미지 분류: MobileNet
  * 포즈 추정: BodyPose (BlazePose 기반)
  * 손 추적: HandPose
  * 얼굴 랜드마크 추적: FaceMesh
  * 텍스트 생성: charRNN
  * 사운드 분류: SoundClassifier
  * 신경망 훈련: NeuralNetwork([ml5][2], [ml5][2], [ml5][3], [MLK - Machine Learning Knowledge][4], [YouTube][5])

* **브라우저 기반 실행**: 별도의 설치 없이 CDN을 통해 바로 사용할 수 있으며, 웹캠이나 마이크와 같은 브라우저 기능과도 쉽게 연동됩니다.

---

### 🚀 최신 업데이트: ml5.js 1.0 (2024년 8월 출시)

2024년 8월에 출시된 ml5.js 1.0은 "next generation"이라는 이름으로 대대적인 개편이 이루어졌습니다. 주요 변경 사항은 다음과 같습니다:([ml5][6])

* **TensorFlow\.js 4.22.0으로 업그레이드**: 최신 버전의 TensorFlow\.js를 기반으로 하여 성능과 호환성이 향상되었습니다.

* **API 정비 및 단순화**: 사용되지 않거나 유지 관리되지 않는 기능을 제거하고, 초보자 친화적인 API로 재설계되었습니다.

* **새로운 모델 도입**:

  * BodyPose: BlazePose 기반의 다중 인체 포즈 추정 모델
  * HandPose: 손가락 관절 추적 모델
  * FaceMesh: 얼굴 랜드마크 추적 모델([ml5][2], [ml5][6])

* **웹사이트 및 문서 개선**: Gatsby와 Docsify를 활용하여 웹사이트를 재구성하고, 초보자도 쉽게 따라할 수 있는 튜토리얼과 예제를 제공하고 있습니다.

---

### 📘 시작하기

ml5.js를 사용하려면 다음과 같이 HTML 파일에 스크립트를 추가하면 됩니다:

```html
<script src="https://unpkg.com/ml5@1.0.0/dist/ml5.min.js"></script>
```



또는 npm을 통해 설치할 수도 있습니다:

```bash
npm install ml5 @tensorflow/tfjs
```



자세한 사용법과 예제는 공식 웹사이트의 [Getting Started](https://ml5js.org/) 페이지를 참고하시기 바랍니다.([ml5 Documentation][7])

---

### 🎓 학습 자료

ml5.js를 활용한 머신러닝 학습을 원하신다면 다음 자료를 추천드립니다:

* **The Coding Train의 ml5.js 강좌**: Daniel Shiffman이 진행하는 유튜브 시리즈로, 이미지 분류, 포즈 추정, 텍스트 생성 등 다양한 주제를 다룹니다.

* **ml5.js 공식 Learn 페이지**: 다양한 튜토리얼과 예제가 제공되며, 커뮤니티에서 공유한 프로젝트도 확인할 수 있습니다.

---
