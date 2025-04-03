## 🎯 그래픽스 기술의 계층화 분류 (Layered Classification)

| **계층** | **정의** | **주요 기술 / 도구** | **응용 및 특징** |
|----------|-----------|-----------------------|------------------|
| 🔧 **저수준 계층**<br> (Low-level API) | GPU를 직접 제어하는 **그래픽스 API**로, 렌더링 파이프라인 조작 가능 | 🔹 **OpenGL**<br>🔹 **Direct3D** (DirectX)<br>🔹 **Vulkan**<br>🔹 **Metal** (Apple)<br>🔹 **WebGPU** (웹 전용) | ✅ 고성능 렌더링 엔진 직접 구현 가능<br>✅ 크로스플랫폼/멀티플랫폼 대응<br>⚠️ 복잡한 셰이더/버퍼/파이프라인 관리 필요 |
| 🧰 **중간 계층**<br> (Toolkit / Support Library) | 그래픽스 API를 보조하며, 입력 처리, 창 생성, GUI, 리소스 로딩 등 기능 제공 | 🔹 **GLFW**, **SDL** – 창, 키보드/마우스 처리<br>🔹 **ImGui** – 실시간 디버그 UI<br>🔹 **Assimp** – 3D 모델 로딩<br>🔹 **stb_image**, **SOIL** – 이미지 로딩<br>🔹 **FreeType** – 폰트 렌더링<br>🔹 **OpenAL**, **FMOD** – 사운드<br>🔹 **OpenCV** – 영상 처리 | ✅ 다양한 그래픽스 응용 개발 시 보조적 사용<br>✅ C/C++과 연동 쉬움<br>✅ 게임엔진 또는 직접 구현 시 필수 |
| 🧱 **고수준 계층**<br> (Framework / Engine) | 그래픽스뿐 아니라 물리, 사운드, 네트워크, 에디터 등을 포괄하는 **통합 개발 환경** | 🔹 **Unity** (C#)<br>🔹 **Unreal Engine** (C++/Blueprint)<br>🔹 **Godot** (GDScript)<br>🔹 **Three.js**, **Babylon.js** (Web)<br>🔹 **Cocos2d-x**, **LÖVE** (2D 게임 엔진)<br>🔹 **Blender Game Engine** (비활성화됨) | ✅ 게임, 시뮬레이션, 가상현실 등 고속 개발<br>✅ 에디터 및 시각화 도구 제공<br>✅ 스크립트 기반 개발<br>⚠️ 내부 렌더링 파이프라인 제어 한계 |
| 🌐 **플랫폼/런타임 계층**<br> (Platform / Runtime Environment) | 그래픽스 API나 엔진이 **실행되는 환경** | 🔹 **Windows** – DirectX<br>🔹 **Linux** – OpenGL/Vulkan<br>🔹 **macOS/iOS** – Metal<br>🔹 **Android** – OpenGL ES/Vulkan<br>🔹 **Web Browsers** – WebGL/WebGPU<br>🔹 **XR 기기** – Meta Quest, SteamVR 등 | ✅ 플랫폼에 따라 지원되는 API 달라짐<br>✅ 앱 배포, 성능 최적화 관점에서 중요 |

---

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
