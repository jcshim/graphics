# 웹(Web)에서 그래픽스 - 계층 구조(Hierarchy)

---

## 🧱 Web 그래픽스 기술 계층 구조

### 🟩 1. **고수준 그래픽스 라이브러리 (High-Level Libraries)**

> 복잡한 수학, 셰이더, 렌더링 코드를 대신 처리해주는 **"간편 도구"**

* **Three.js**: WebGL 기반 3D 그래픽스 대표 라이브러리

  * 예: `scene`, `camera`, `mesh` 등 객체 기반 조작
* **Babylon.js**: 게임 개발에 강한 고성능 엔진
* **PlayCanvas**: 웹기반 게임 엔진, 에디터 포함
* **PixiJS**: 고성능 2D 렌더링 라이브러리
* **Zdog, Two.js**: 간단한 2D/3D 아트워크 생성

✅ **특징**: 쉬운 사용, 빠른 개발, 입문자 및 프로토타이핑에 유리

---

### 🟨 2. **중간 수준 프레임워크 / 브릿지 (Bridge Layer / Mid-Level)**

> **고수준 ↔ 저수준** 사이를 연결하는 중간 계층
> 아직은 많지 않지만, WebGPU 보급과 함께 증가 추세

* **wgpu / WebGPU Starter Libs**: WebGPU를 쉽게 쓰도록 만든 유틸리티
* **Three.js WebGPURenderer**: Three.js를 WebGPU로 렌더링하는 실험적 엔진
* **regl / twgl.js**: WebGL을 함수형 스타일로 쉽게 사용할 수 있도록 도와줌

✅ **특징**: 코드 유연성 증가, 라이브러리 없이 직접 구현도 가능

---

### 🟥 3. **저수준 그래픽 API (Low-Level API)**

> 브라우저가 제공하는 **GPU 제어를 위한 공식 API**

* **WebGL 1.0 / 2.0**

  * OpenGL ES 2.0/3.0 기반
  * 폭넓게 지원됨 (모든 브라우저 호환)
* **WebGPU**

  * Vulkan, Metal, Direct3D12 기반
  * 최신 브라우저에서 점진적 지원 중 (2023년부터)

✅ **특징**: 성능과 제어력 높음, 사용 난이도 높음, 셰이더 작성 필요

---

### 🟦 4. **하드웨어 및 브라우저 엔진 (플랫폼 레벨)**

> 실제 GPU와 통신하고 명령을 실행하는 **브라우저 내부/OS단**

* **GPU Driver / Graphics Driver** (NVIDIA, AMD, Intel 등)
* **브라우저 렌더링 엔진**

  * Chromium → ANGLE(WebGL), Dawn(WebGPU)
  * Firefox → WebRender
  * Safari → Metal 백엔드

✅ **특징**: 사용자가 직접 제어하지 않지만, 성능과 호환성에 영향

---

## 📚 계층 구조 요약 그림

```
[ 사용자의 애플리케이션 코드 ]
        ▲
[ Three.js / Babylon.js / PixiJS ]     ← 고수준
        ▲
[ WebGL / WebGPU 유틸리티 ]           ← 중간 수준
        ▲
[ WebGL / WebGPU API ]                ← 저수준
        ▲
[ GPU 드라이버 / 브라우저 렌더 엔진 ]  ← 하드웨어/플랫폼
```

---

## 🎯 정리

| 계층  | 예시                      | 특징                  |
| --- | ----------------------- | ------------------- |
| 고수준 | Three.js, Babylon.js    | 쉽고 빠른 개발, 학습용 적합    |
| 중간층 | regl, twgl.js, wgpu     | API 추상화, 커스터마이징 유리  |
| 저수준 | WebGL, WebGPU           | 고성능, 직접 제어, 복잡함     |
| 플랫폼 | ANGLE, Dawn, GPU 드라이버 등 | 사용자 제어 불가, 성능 결정 요소 |
