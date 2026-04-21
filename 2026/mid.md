1. **정의:** 컴퓨터를 사용하여 이미지나 영상을 생성, 조작, 저장 및 표시하는 기술.
2. **응용:** 영화/애니메이션, 비디오 게임, CAD(설계), 의료 영상(MRI/CT), 가상현실(VR/AR), 교육용 시뮬레이션 등.
3. **Graphics API** (또는 그래픽스 프로그래밍 인터페이스)
4. 
   1) **Scene (장면)**
   2) **Camera (카메라)** / **Perspective (원근)**, **Orthographic (직교)**
   3) **Renderer (렌더러)** / **Geometry (지오메트리)**, **Material (재질)**
5. **Vertex** Processing / **Rasterization** (래스터화) / **Fragment** Processing
6. **GLSL** / **TSL** (Three.js Shading Language)
7. 1세대: **OpenGL** / 2세대: **Metal, Vulkan, Direct**X 12
8. **WebGL** / **WebGPU**
9. **Max** / **Maya** / **Blender** / **Editor**
10. **glTF / GLB** / **OBJ** / **FBX**
11. **Three.js 코딩 (예시):**
```javascript
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshNormalMaterial();
const cube = new THREE.Mesh(geometry, material);

// 45도는 라디안으로 Math.PI / 4
cube.rotation.x = Math.PI / 4;
cube.rotation.y = Math.PI / 4;

scene.add(cube);
camera.position.z = 5;

function animate() {
    requestAnimationFrame(animate);
    renderer.render(scene, camera);
}
animate();
```
12. **p5.js 코딩 (예시):**
```javascript
function setup() {
  createCanvas(400, 400, WEBGL);
}

function draw() {
  background(200);
  orbitControl(); // 마우스 드래그 회전
  normalMaterial(); // 법선 재질
  box(120); // 크기 120 정육면체
}
```
