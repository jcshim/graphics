# Graphics with Deep Learning

---

## 🎯 1. ml5.js + p5.js
💡 브라우저 기반 시각적 딥러닝 체험 - 초중급용

---

### ✅ **활용 예제**:

* **포즈 인식 인터랙션**: webcam을 통해 사람의 자세(PoseNet)를 인식하고, p5.js로 움직이는 캐릭터에 반영
* **이미지 분류 게임**: Teachable Machine 모델을 활용한 실시간 이미지 분류 → p5.js로 미니게임 구성
* **스타일 전이 데모**: Van Gogh 스타일을 webcam 영상에 실시간 적용 (Fast Style Transfer)
* **얼굴 메쉬 시각화**: `faceMesh` 모델을 통해 감지한 얼굴 랜드마크를 삼각형 메쉬로 표현 → 3D 느낌의 얼굴 추적 인터페이스 구현 가능

---

### ✅ **학습 효과**:

* 딥러닝의 작동 방식(입력 → 모델 → 출력)을 시각적으로 이해
* JavaScript 코드만으로 모델을 불러오고 실행하는 구조 학습
* 데이터 기반의 그래픽 변화 이해
* **얼굴 랜드마크 정보의 구조와 활용 방법** 습득 (예: 얼굴 추적, 마스크 필터, 이모지 적용 등)

### ✅ **코드 예시 (FaceMesh)**:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>ml5.js faceMesh Triangle Mesh Example</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.9.4/p5.min.js"></script>
    <script src="https://unpkg.com/ml5@1/dist/ml5.min.js"></script>
  </head>
  <body>
    <script src="sketch.js"></script>
  </body>
</html>
```

```javascript
/*
 * 👋 Hello! This is an ml5.js example made and shared with ❤️.
 * Learn more about the ml5.js project: https://ml5js.org/
 * ml5.js license and Code of Conduct: https://github.com/ml5js/ml5-next-gen/blob/main/LICENSE.md
 *
 * This example demonstrates drawing a triangular mesh using ml5.faceMesh.
 */

let faceMesh;
let video;
let faces = [];
let options = { maxFaces: 1, refineLandmarks: false, flipHorizontal: false };
let triangles;

function preload() {
  // Load the faceMesh model
  faceMesh = ml5.faceMesh(options);
}

function setup() {
  createCanvas(640, 480);
  // Create the webcam video and hide it
  video = createCapture(VIDEO);
  video.size(640, 480);
  video.hide();
  // Load the triangle indices for drawing the mesh
  triangles = faceMesh.getTriangles();
  // Start detecting faces from the webcam video
  faceMesh.detectStart(video, gotFaces);
}

function draw() {
  // Draw the webcam video
  image(video, 0, 0, width, height);

  // Draw all the triangles
  for (let i = 0; i < faces.length; i++) {
    let face = faces[i];
    for (let j = 0; j < triangles.length; j++) {
      let indices = triangles[j];
      let pointAIndex = indices[0];
      let pointBIndex = indices[1];
      let pointCIndex = indices[2];
      let pointA = face.keypoints[pointAIndex];
      let pointB = face.keypoints[pointBIndex];
      let pointC = face.keypoints[pointCIndex];

      noFill();
      stroke(0, 0, 255);
      strokeWeight(1);
      triangle(pointA.x, pointA.y, pointB.x, pointB.y, pointC.x, pointC.y);
    }
  }
}

// Callback function for when faceMesh outputs data
function gotFaces(results) {
  // Save the output to the faces variable
  faces = results;
}

```

---

### 🎯 2. **TensorFlow\.js + Three.js**

> 💡 *고급 인터랙티브 3D + 딥러닝 모델 연동*

#### ✅ 활용 예제:

* **3D 얼굴 제스처 제어**: 얼굴 인식 모델 → 3D 캐릭터의 표정이나 방향 변화
* **손 제스처로 3D 물체 회전**: 손 위치/포즈 인식 → Three.js 객체에 rotation 적용
* **GAN 이미지 생성 결과를 3D 환경에 반영**: TensorFlow\.js로 이미지 생성 → 3D plane texture로 활용

#### ✅ 학습 효과:

* WebGL 기반 3D 렌더링과 머신러닝 inference 연결 학습
* 실시간 처리와 브라우저 내 GPU 연산의 역할 체험
* 고급 그래픽스(Three.js)와 AI 기술의 융합 경험

#### ✅ 프로젝트 제안:

* `“AI Mirror”: 카메라 기반 얼굴 인식 → 3D 아바타 따라하기`
* `“Smart Gallery”: AI가 생성한 그림이 3D 전시관에 자동 배치됨`

---

### 🎯 3. **ONNX.js**

> 💡 *Python 수업과 그래픽스 수업을 연결하는 다리*

#### ✅ 활용 예제:

* **PyTorch로 학습한 분류기 모델 → ONNX 변환 → 웹에서 실행**
* **음성 감정 인식 결과에 따라 캐릭터 표정 변화**
* **손글씨 인식 결과를 2D/3D 캔버스에 실시간 반영**

#### ✅ 학습 효과:

* Python에서 만든 모델을 JavaScript로 활용하는 전체 파이프라인 이해
* ONNX 모델 포맷과 교차 플랫폼 학습
* 백엔드(모델 학습) + 프론트엔드(그래픽 렌더링) 분리형 실습 가능

#### ✅ 구성 예시:

1. Python에서 PyTorch → ONNX 변환
2. `onnxjs`로 모델 로드
3. canvas/three.js에서 시각적 결과 표현

---

### ✅ 종합 정리

| 조합                        | 난이도   | 그래픽스 활용      | 추천 실습 주제              |
| ------------------------- | ----- | ------------ | --------------------- |
| ml5.js + p5.js            | ★☆☆☆☆ | 2D 인터랙션, 이미지 | 포즈 추적, 분류 게임, 스타일 전이  |
| TensorFlow\.js + Three.js | ★★★★☆ | 3D 렌더링 연동    | 3D 캐릭터 제어, GAN 시각화    |
| ONNX.js                   | ★★★☆☆ | 외부 학습 모델 연동  | PyTorch 모델 시각화, 감정 표현 |
