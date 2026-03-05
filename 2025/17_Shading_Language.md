# 셰이딩 언어(Shading Language)


---

## 🎓 대학 그래픽스 수업별 Shading Language 비교표

| 학습 대상 API  | 주요 셰이딩 언어                               | 특징 및 설명                                                             | 사용 방식                              |
| ---------- | --------------------------------------- | ------------------------------------------------------------------- | ---------------------------------- |
| **OpenGL** | **GLSL (OpenGL Shading Language)**      | C 스타일의 고급 셰이딩 언어. 가장 널리 쓰이며, 다양한 자료와 예제가 존재.                        | 직접 작성 (`.vert`, `.frag` 등)         |
| **WebGL**  | **GLSL ES (GLSL for Embedded Systems)** | WebGL용 경량 버전. 정밀도 선언 필요 (`precision mediump float;` 등). 브라우저에서 실행됨. | 직접 작성 또는 Three.js에서 삽입             |
| **WebGPU** | **WGSL (WebGPU Shading Language)**      | WebGPU의 공식 셰이딩 언어. 정적 타입, Rust/C 스타일 혼합 문법. SPIR-V 대체.              | 직접 작성 (`.wgsl`)                    |
| **Vulkan** | **GLSL → SPIR-V** 또는 **HLSL → SPIR-V**  | Vulkan은 셰이더를 **SPIR-V** 중간 코드로 받아들이므로, GLSL 또는 HLSL로 작성 후 컴파일 필요    | 직접 작성 후 `glslangValidator` 등으로 컴파일 |

---

## ✅ 각 API별 상세 설명

### 🔷 OpenGL + GLSL

* **언어**: `GLSL`
* **버전**: `#version 330`, `#version 450` 등
* **특징**:

  * 고급 기능 지원 (`geometry`, `tessellation`, `compute shader`)
  * `layout(location=...)` 등 Vulkan 문법과 호환되는 확장 가능
* **교육 적용**: 기초 그래픽스 이론 및 셰이더 개념 학습에 적합

---

### 🔷 WebGL + GLSL ES

* **언어**: `GLSL ES`
* **버전**: `#version 100`, `#version 300 es`
* **특징**:

  * 제한된 정밀도(`lowp`, `mediump`, `highp`)
  * `gl_FragColor`, `attribute`, `varying` 등 레거시 문법
* **교육 적용**: Three.js 실습, 웹 시각화, 인터랙티브 그래픽에 적합

---

### 🔷 WebGPU + WGSL

* **언어**: `WGSL (WebGPU Shading Language)`
* **버전**: 버전 명시 없음 (정적 구조 기반)
* **특징**:

  * `@binding`, `@location`, `@group` 등 메타 정보 명시
  * 강한 정적 타이핑, Rust/C 스타일 문법
* **교육 적용**: 차세대 웹 그래픽스 및 GPU-Native 연산 교육에 적합

예시:

```wgsl
@group(0) @binding(0) var<uniform> myMatrix: mat4x4<f32>;

@vertex
fn vs_main(@location(0) pos: vec3<f32>) -> @builtin(position) vec4<f32> {
  return myMatrix * vec4<f32>(pos, 1.0);
}
```

---

### 🔷 Vulkan + GLSL or HLSL → SPIR-V

* **언어**:

  * `GLSL (with Vulkan extensions)`
  * 또는 `HLSL (compiled to SPIR-V)`
* **중간 코드**: `SPIR-V` (Standard Portable Intermediate Representation)
* **특징**:

  * 셰이더는 `.spv` 바이너리로 Vulkan에 로딩
  * `glslangValidator`, `shaderc`, `dxc` 등의 툴 사용
* **교육 적용**: 고급 렌더링 기법, GPU 파이프라인 설계 수업에 적합

예시 컴파일:

```bash
glslangValidator -V shader.vert -o shader.vert.spv
```

---

## ✅ 요약 그래프

```
GLSL      → [OpenGL]
GLSL ES   → [WebGL]
WGSL      → [WebGPU]
GLSL/HLSL → [SPIR-V] → [Vulkan]
```

---

## ✅ 결론

> 대학 그래픽스 교육에서 셰이딩 언어는 **API에 따라 다르며**, 각 언어는 목적과 문법, 학습 난이도에 차이가 있습니다.
> 실용적인 교육 설계를 위해 다음과 같이 구성하는 것을 권장합니다:

* **GLSL**: OpenGL 수업의 중심
* **GLSL ES**: 웹/Three.js 중심 실습
* **WGSL**: WebGPU 기반 최신 기술 체험
* **GLSL→SPIR-V**: Vulkan 기반 고급 그래픽스 수업
