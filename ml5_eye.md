# ml5_eye.js
![image](https://github.com/user-attachments/assets/60f602b3-4113-4702-a018-3a708eeaaa8d)

### index.js
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>ml5.js faceMesh Webcam Example</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.9.4/p5.min.js"></script>
    <script src="https://unpkg.com/ml5@1/dist/ml5.min.js"></script>
  </head>
  <body>
    <script src="sketch.js"></script>
  </body>
</html>

```
### sketch.js
```
let faceMesh;
let video;
let faces = [];
let options = { maxFaces: 1, refineLandmarks: false, flipHorizontal: false };

// 눈 keypoint 인덱스 (왼쪽 + 오른쪽 눈 주요 점들)
const eyeIndices = [
  33, 133, 160, 159, 158, 157, 173, 144, 145, 153, // 왼쪽 눈
  362, 263, 387, 386, 385, 384, 398, 373, 374, 380 // 오른쪽 눈
];

function preload() {
  faceMesh = ml5.faceMesh(options);
}

function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.size(640, 480);
  video.hide();
  faceMesh.detectStart(video, gotFaces);
}

function draw() {
  image(video, 0, 0, width, height);

  for (let i = 0; i < faces.length; i++) {
    let face = faces[i];

    for (let idx of eyeIndices) {
      if (face.keypoints[idx]) {
        let keypoint = face.keypoints[idx];
        fill(0, 255, 0);
        noStroke();
        circle(keypoint.x, keypoint.y, 5);
      }
    }
  }
}

function gotFaces(results) {
  faces = results;
}
```
![image](https://github.com/user-attachments/assets/d4904de9-bc5c-4d22-bc67-5e08343d3d11)
```
let faceMesh;
let video;
let faces = [];
let blinkCount = 0;
let blinkThreshold = 0.23; // EAR이 이 값보다 낮으면 눈 감김
let blinkCooldown = false; // 중복 계산 방지
let options = { maxFaces: 1, refineLandmarks: false, flipHorizontal: false };

function preload() {
  faceMesh = ml5.faceMesh(options);
}

function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.size(640, 480);
  video.hide();
  faceMesh.detectStart(video, gotFaces);
  textSize(24);
  fill(255, 0, 0);
}

// EAR 계산 함수 (좌우 눈 모두 사용 가능)
function getEAR(p1, p2, p3, p4, p5, p6) {
  const vert1 = dist(p2.x, p2.y, p6.x, p6.y);
  const vert2 = dist(p3.x, p3.y, p5.x, p5.y);
  const hor = dist(p1.x, p1.y, p4.x, p4.y);
  return (vert1 + vert2) / (2.0 * hor);
}

function draw() {
  image(video, 0, 0, width, height);

  if (faces.length > 0) {
    let keypoints = faces[0].keypoints;

    // 왼쪽 눈 EAR 계산 (indices: 33, 160, 158, 133, 153, 144)
    let earLeft = getEAR(
      keypoints[33], keypoints[160], keypoints[158],
      keypoints[133], keypoints[153], keypoints[144]
    );

    // 오른쪽 눈 EAR 계산 (indices: 362, 387, 385, 263, 380, 373)
    let earRight = getEAR(
      keypoints[362], keypoints[387], keypoints[385],
      keypoints[263], keypoints[380], keypoints[373]
    );

    let ear = (earLeft + earRight) / 2;

    // 깜박임 감지
    if (ear < blinkThreshold && !blinkCooldown) {
      blinkCount++;
      blinkCooldown = true;
      setTimeout(() => {
        blinkCooldown = false;
      }, 300); // 0.3초 쿨다운
    }

    // 눈에 초록 점 찍기
    const eyeIndices = [
      33, 133, 160, 159, 158, 157, 173, 144, 145, 153,
      362, 263, 387, 386, 385, 384, 398, 373, 374, 380
    ];
    for (let idx of eyeIndices) {
      if (keypoints[idx]) {
        let kp = keypoints[idx];
        fill(0, 255, 0);
        noStroke();
        circle(kp.x, kp.y, 5);
      }
    }
  }

  // 깜박임 횟수 표시
  fill(255, 0, 0);
  text("Blink count: " + blinkCount, 20, 30);
}

function gotFaces(results) {
  faces = results;
}
```

