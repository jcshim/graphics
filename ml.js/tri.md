
https://editor.p5js.org/ml5/sketches/8mT3aRjUe

```
function setup() {
  createCanvas(640, 480);
  // Create the webcam video and hide it

  navigator.mediaDevices.enumerateDevices().then((devices) => {
    let videoDevices = devices.filter((device) => device.kind === "videoinput");

    if (videoDevices.length >= 2) {
      let constraints = {
        video: {
          deviceId: { exact: videoDevices[1].deviceId },
          width: 640,
          height: 480,
        },
      };

      video = createCapture(constraints, () => {
        video.size(640, 480);
        video.hide();
        faceMesh.detectStart(video, gotFaces);
        triangles = faceMesh.getTriangles(); // <-- 여기를 추가!
      });
    } else {
      console.error("두 번째 카메라가 없습니다.");
    }
  });
}
```
