# 🎨 셰이더 언어(Shader Language)란?

> **빛**, **그림자**, **질감**, **색상** 등 시각적인 효과를 **GPU**에서 처리하기 위한 **그래픽 전용 프로그래밍 언어**입니다.

3D 모델이 더 **현실감 있게 보이도록** 만드는 마법 같은 도구!  
예: 금속 표면 반짝임, 유리의 투명함, 그림자의 흐릿함 등 ✨

---

## 🌈 셰이더 언어의 대표 종류

### ① GLSL  
**(OpenGL Shading Language)**  
- 🔧 **사용 환경**: OpenGL, WebGL  
- 💡 **특징**:  
  - C언어와 비슷한 문법  
  - 다양한 플랫폼에서 사용 가능 (Windows, Mac, Linux, Web 등)  
- 🎯 **활용 예시**: 웹 3D 그래픽, 모바일 게임, 인터랙티브 아트  
- 📦 **간단한 예제**
  ```glsl
  void main() {
      gl_FragColor = vec4(1.0, 0.5, 0.0, 1.0); // 주황색 출력
  }
  ```

---

### ② HLSL  
**(High-Level Shading Language)**  
- 🔧 **사용 환경**: DirectX (Windows 전용)  
- 💡 **특징**:  
  - 마이크로소프트에서 개발  
  - 윈도우 게임/그래픽에 최적화  
- 🎯 **활용 예시**: PC 게임, Xbox 콘솔 게임  
- 📦 **간단한 예제**
  ```hlsl
  float4 main() : SV_Target {
      return float4(1.0, 0.5, 0.0, 1.0); // 주황색 출력
  }
  ```

---

### ③ SPIR-V  
**(Standard Portable Intermediate Representation)**  
- 🔧 **사용 환경**: Vulkan, OpenCL  
- 💡 **특징**:  
  - **중간 코드 형식** (개발자가 직접 작성하진 않음)  
  - GLSL/HLSL을 컴파일하면 나오는 **GPU 실행용 언어**  
- 🎯 **활용 예시**: 고성능 게임, VR, 복잡한 그래픽 연산  
- 🔄 **흐름**
  ```
  GLSL or HLSL 코드 → SPIR-V로 컴파일 → Vulkan에서 실행
  ```

---

## 🧩 한눈에 비교

| 언어     | 사용 환경         | 특징                             | 활용 분야                  |
|----------|------------------|----------------------------------|---------------------------|
| **GLSL** | OpenGL, WebGL    | C 유사 문법, 크로스 플랫폼       | 웹, 모바일, 일반 그래픽    |
| **HLSL** | DirectX (Windows)| MS 전용 문법, DirectX 최적화     | PC/Xbox 게임               |
| **SPIR-V** | Vulkan, OpenCL  | 중간 코드, GLSL/HLSL의 타겟 언어 | 고성능 그래픽, 최신 기술   |

---

## 🎁 요약 한 줄 정리
> **GLSL과 HLSL은 사람이 쓰는 셰이더 코드, SPIR-V는 그것들을 GPU가 실행할 수 있게 바꾼 중간 코드!**

---

더 궁금한 게 있거나 셰이더 실습 예제도 보고 싶다면 언제든지 말해줘요 😄
