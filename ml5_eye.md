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

