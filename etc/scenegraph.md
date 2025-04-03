## 🌳 Three.js 씬 그래프란?

Three.js는 `Object3D` 기반의 **트리 구조**를 사용합니다.  
모든 3D 객체(`Mesh`, `Camera`, `Light`, `Group`, `Scene` 등)는 `Object3D`를 상속하며, **부모-자식 관계**로 연결됩니다.

### ✅ 주요 특징
- 계층 구조: 트리(tree)처럼 연결됨
- 부모의 위치/회전/스케일이 자식에게 영향을 줌
- `scene`이 루트이며, 렌더링 시 이 루트를 기준으로 탐색

---

## 🔍 예시 구조: 조금 더 복잡한 씬 그래프

```
scene (THREE.Scene)
├── group (THREE.Group)
│   ├── mesh1 (THREE.Mesh)
│   │   ├── geometry: BoxGeometry
│   │   └── material: MeshStandardMaterial
│   └── mesh2 (THREE.Mesh)
│       ├── geometry: SphereGeometry
│       └── material: MeshStandardMaterial
├── directionalLight (THREE.DirectionalLight)
├── ambientLight (THREE.AmbientLight)
└── camera (THREE.PerspectiveCamera) ← 보통은 scene에 넣지 않지만 가능
```

---

## ⚙️ 씬 그래프 구성 객체

| 타입               | 설명 |
|--------------------|------|
| `Scene`            | 최상위 루트 컨테이너 |
| `Group`            | 여러 객체를 묶는 컨테이너 (자식 포함 가능) |
| `Mesh`             | geometry + material 로 구성된 렌더링 가능한 객체 |
| `Light`            | 조명: Directional, Ambient, Point, Spot 등 |
| `Camera`           | 뷰포인트 정의 (보통 scene에 포함되지 않음) |
| `Object3D`         | 모든 객체의 기반 클래스 |
| `Helper`           | 시각화 도구 (e.g., GridHelper, AxesHelper) |

---

## 🧠 렌더링 동작 방식

렌더러는 다음 순서로 작동합니다:

1. `renderer.render(scene, camera)` 호출
2. `scene`을 루트로 **모든 자식 노드들을 순회**
3. 각 객체의 변환 행렬 계산
4. **Mesh만 렌더링**, Light/Camera는 설정에 사용됨
5. `camera` 시점에서 최종적으로 WebGL 화면에 출력

---

## 🎨 예시 코드로 구성된 씬 그래프 (구체적 객체까지)

```js
const scene = new THREE.Scene();
const group = new THREE.Group();
scene.add(group);

const mesh1 = new THREE.Mesh(
  new THREE.BoxGeometry(),
  new THREE.MeshStandardMaterial({ color: 0xff0000 })
);
const mesh2 = new THREE.Mesh(
  new THREE.SphereGeometry(),
  new THREE.MeshStandardMaterial({ color: 0x00ff00 })
);
group.add(mesh1);
group.add(mesh2);

const light = new THREE.DirectionalLight(0xffffff, 1);
scene.add(light);

const camera = new THREE.PerspectiveCamera(75, aspect, 0.1, 1000);
```

이 구조는 다음과 같은 그래프가 됩니다:

```
scene
├── group
│   ├── mesh1 (Box)
│   └── mesh2 (Sphere)
└── light
```

---

## 📦 더 심화 탐색: Object3D 메서드

- `.add(object)` – 자식 추가
- `.remove(object)` – 자식 제거
- `.traverse(callback)` – 씬 그래프 전체 순회
- `.parent` – 부모 객체 참조
- `.children` – 자식 배열

```js
scene.traverse(obj => {
  console.log(obj.type); // 'Mesh', 'Group', 'DirectionalLight', ...
});
```

---

## 📌 요약

| 개념 | 설명 |
|------|------|
| **씬 그래프** | 트리 구조로 씬을 구성 |
| **계층화** | 부모가 자식에 영향 (변환/위치 등) |
| **Object3D** | 모든 객체의 기반 클래스 |
| **traverse()** | 전체 그래프를 탐색할 수 있는 핵심 함수 |

---
