## 🎯 그래픽스 기술의 계층화 분류 (Layered Classification)

| **계층** | **정의** | **주요 기술 / 도구** | **응용 및 특징** |
|----------|----------|-----------------------|------------------|
| 🔧 **저수준 계층**<br>(Low-level API) | GPU를 직접 제어하는 렌더링 API | OpenGL, Direct3D, Vulkan, Metal, WebGPU, WebGL | ✅ 고성능 렌더링 엔진 개발 가능, ✅ 크로스플랫폼 지원, ⚠️ 셰이더·파이프라인 등 저수준 구현 필요 |
| 🧰 **중간 계층**<br>(Toolkit / Library) | API를 보조하는 입출력·UI·로딩 도구 | GLFW, SDL, ImGui, Assimp, stb_image, FreeType, OpenAL, OpenCV | ✅ 창 생성, 입력 처리, UI 구현 등 보조 기능 제공, ✅ 엔진 없이 직접 구현할 때 유용, ✅ 다른 계층과 유연하게 결합 가능 |
| 🧱 **고수준 계층**<br>(Framework / Engine) | 그래픽·물리·사운드 등 통합 개발 환경 | Unity, Unreal, Godot, Three.js, Babylon.js, Cocos2d-x, LÖVE | ✅ 빠른 프로토타이핑 및 배포, ✅ 시각적 에디터 제공, ⚠️ 렌더링 세부 제어에는 한계 있음 |
| 🌐 **플랫폼 계층**<br>(Platform / Runtime) | API와 엔진이 동작하는 기반 환경 | Windows, Linux, macOS, Android, Web, XR 기기 | ✅ 플랫폼에 따라 지원 API 상이, ✅ 앱 최적화와 성능에 영향, ✅ 디바이스 호환성 고려 필요 |

## 🧭 시나리오별 응용 예시

| 시나리오 | 활용 계층 | 기술 예시 |
|----------|-----------|-----------|
| **자체 렌더링 엔진 개발** | 저수준 + 중간 계층 | OpenGL + GLFW + ImGui |
| **크로스 플랫폼 게임 개발** | 고수준 계층 | Unity 또는 Unreal |
| **웹 기반 3D 뷰어 제작** | 고수준 + 저수준 | Three.js (WebGL 기반) |
| **머신러닝 + 영상처리** | 중간 계층 | OpenCV + OpenGL |
| **교육용 시각화 도구** | 중간 + 고수준 | SDL + ImGui or Godot |
| **실시간 UI 디버깅 툴** | 중간 계층 | ImGui + GLFW |

---

## 🔍 시각적 정리 구조 (텍스트 기반)
```
[플랫폼 계층]
 └── **Windows**, Linux, Android, Web...

[고수준 프레임워크/엔진]
 └── Unity, Unreal, **Three.js**, Godot...

[중간 계층 라이브러리/툴킷]
 └── SDL, GLFW, ImGui, OpenCV, Assimp...

[저수준 그래픽스 API]
 └── OpenGL, DirectX, Vulkan, Metal, **WebGL**, WebGPU...
```
