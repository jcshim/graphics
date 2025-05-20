# ✅ OpenGL이란?

**OpenGL**은 **2D 및 3D 그래픽을 그리기 위한 표준 API(Application Programming Interface)**입니다.
1980년대 후반 실리콘 그래픽스(SGI)가 개발했으며, 지금도 다양한 플랫폼(Windows, macOS, Linux 등)에서 널리 사용되고 있습니다.

---

### ✅ 주요 특징

1. **플랫폼 독립적**: Windows, macOS, Linux 등 다양한 운영체제에서 사용 가능
2. **하드웨어 가속**: GPU를 사용해 빠른 그래픽 처리 가능
3. **표준화된 API**: 다양한 그래픽 카드에서 동일한 코드를 사용할 수 있음
4. **C 기반**: 함수 호출 방식이 C언어 스타일이라 초보자도 구조를 익히기 쉬움

---

### ✅ OpenGL로 할 수 있는 것

* 삼각형, 사각형 등 기본 도형 그리기
* 텍스처(Texture) 입히기
* 조명 효과, 그림자, 반사 등 시각 효과
* 3D 모델링 및 카메라 시점 구현
* 게임, 시뮬레이션, CAD 등 그래픽 기반 응용 프로그램 제작

---

### ✅ OpenGL을 배우기 전에 알면 좋은 개념

* **좌표계**: x, y, z축 개념
* **벡터와 행렬**: 이동, 회전, 확대/축소 등에 사용
* **렌더링 파이프라인**: 정점 처리 → 래스터화 → 픽셀 처리
* **셰이더(Shader)**: GPU에서 실행되는 코드로, 화면의 색과 모양을 조절

---

### ✅ 초보자에게 추천하는 학습 순서

1. **기초 문법**: OpenGL 함수 사용법 익히기 (C/C++ 기반)
2. **도형 그리기**: 삼각형, 사각형 등 기본 도형 출력
3. **색상 및 변환**: 색 바꾸기, 회전/이동 등 적용
4. **카메라와 투영**: 원근감 있는 3D 장면 만들기
5. **셰이더 언어(GLSL)** 학습: 고급 효과 구현

---

### ✅ 참고 도구/라이브러리

* **GLFW**: 윈도우 생성 및 입력 처리
* **GLAD**: OpenGL 함수 로더
* **GLM**: 수학 연산용 라이브러리 (벡터, 행렬 등)
* **FreeGLUT**: 간단한 OpenGL 학습용 유틸리티 툴킷

---

### ✅ 예시 코드 (삼각형 출력)

```cpp
glBegin(GL_TRIANGLES);
glColor3f(1.0f, 0.0f, 0.0f); glVertex3f(0.0f, 1.0f, 0.0f); // 꼭짓점1
glColor3f(0.0f, 1.0f, 0.0f); glVertex3f(-1.0f, -1.0f, 0.0f); // 꼭짓점2
glColor3f(0.0f, 0.0f, 1.0f); glVertex3f(1.0f, -1.0f, 0.0f); // 꼭짓점3
glEnd();
```

# 예제
```
#include <windows.h>
#include <GL/gl.h>
#pragma comment(lib, "opengl32.lib")

LRESULT CALLBACK WndProc(HWND h, UINT m, WPARAM w, LPARAM l) {
    if (m == WM_DESTROY) PostQuitMessage(0);
    return DefWindowProc(h, m, w, l);
}

int WINAPI WinMain(HINSTANCE h, HINSTANCE, LPSTR, int) {
    WNDCLASS wc = { CS_OWNDC, WndProc, 0, 0, h, 0, 0, 0, 0, L"GL" };
    RegisterClass(&wc);
    HWND win = CreateWindow(L"GL", 0, WS_OVERLAPPEDWINDOW | WS_VISIBLE, 100, 100, 800, 600, 0, 0, h, 0);
    HDC dc = GetDC(win);

    PIXELFORMATDESCRIPTOR pfd = { sizeof(pfd), 1, PFD_DRAW_TO_WINDOW | PFD_SUPPORT_OPENGL | PFD_DOUBLEBUFFER };
    SetPixelFormat(dc, ChoosePixelFormat(dc, &pfd), &pfd);
    HGLRC rc = wglCreateContext(dc);
    wglMakeCurrent(dc, rc);

    glMatrixMode(GL_PROJECTION); glLoadIdentity(); glOrtho(-1, 1, -1, 1, -1, 1);

    MSG msg = {};
    while (msg.message != WM_QUIT) {
        if (PeekMessage(&msg, 0, 0, 0, PM_REMOVE)) DispatchMessage(&msg);
        glClear(GL_COLOR_BUFFER_BIT);
        glColor3f(0, 1, 0);
        glBegin(GL_QUADS);
        glVertex2f(-0.5f, -0.5f); glVertex2f(0.5f, -0.5f);
        glVertex2f(0.5f, 0.5f); glVertex2f(-0.5f, 0.5f);
        glEnd();
        SwapBuffers(dc);
        Sleep(16);
    }

    wglMakeCurrent(0, 0); wglDeleteContext(rc); ReleaseDC(win, dc); DestroyWindow(win);
    return 0;
}
```
