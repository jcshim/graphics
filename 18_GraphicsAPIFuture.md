# ✅ **그래픽스 API (Graphics API, Graphics Application Programming Interface)**

---

### 🔹 보다 구체적으로는:

| 용어                          | 설명                                                                                        |
| --------------------------- | ----------------------------------------------------------------------------------------- |
| **Low-level Graphics API**  | GPU 하드웨어에 가까운 수준에서 동작하며, 개발자가 그래픽 파이프라인을 직접 제어할 수 있도록 해줌.<br>예: Vulkan, DirectX 12, Metal |
| **High-level Graphics API** | 비교적 추상화된 방식으로 그래픽을 다루며, 사용하기 쉬움.<br>예: OpenGL, WebGL                                      |
| **브라우저 기반 Graphics API**    | 웹에서 2D/3D 그래픽을 처리할 수 있도록 설계된 API.<br>예: WebGL, WebGPU                                     |

---

### 🔸 관련 상위 분류:

* **렌더링 엔진(Rendering Engine)**: Unity, Unreal 등이 내부적으로 위 API들을 활용해 그래픽을 처리
* **멀티미디어/게임 프레임워크**: SDL, SFML, raylib 등은 이들 API 위에 구축됨

---
## 5년 후의 전망

| API            | 주요 플랫폼                | 향후 5년 전망 (2025\~2030)    | 비고                        |
| -------------- | --------------------- | ------------------------ | ------------------------- |
| **OpenGL**     | 크로스 플랫폼(레거시)          | 🔻 감소 (축소 및 유지용)         | 최신 개발에서 점차 제외, 학습용/레거시용   |
| **DirectX 12** | Windows/Xbox          | 🔼 유지 및 점진 발전            | 고성능 게임 및 Xbox 주력 API      |
| **Metal**      | Apple 전용 (macOS, iOS) | 🔼 안정적 유지, Apple 생태계 확장  | Apple 생태계 내 표준 그래픽 API    |
| **Vulkan**     | 크로스 플랫폼               | 🔼 확대 중 (산업용, 게임, 교육 등)  | 차세대 크로스 플랫폼 표준으로 자리잡는 중   |
| **WebGL**      | 브라우저                  | 🔻 점진적 감소, WebGPU로 전환 예정 | 보편적이지만 한계 있음              |
| **WebGPU**     | 브라우저                  | 🔼 빠르게 성장, Web 표준화 중     | WebGL 대체, 고성능 Web 그래픽의 핵심 |

### 간략 해석:

* 🔼 = 성장 또는 유지
* 🔻 = 감소 또는 레거시화 진행 중
