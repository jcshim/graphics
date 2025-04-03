```
#include <windows.h>
#include <GL/gl.h>
#pragma comment(lib, "opengl32.lib")

LRESULT CALLBACK WndProc(HWND h, UINT m, WPARAM w, LPARAM l) {
    if (m == WM_DESTROY) PostQuitMessage(0);
    return DefWindowProc(h, m, w, l);
}

int main() {
    WNDCLASS wc = { CS_OWNDC, WndProc, 0, 0, GetModuleHandle(NULL), 0, 0, 0, 0, "GLWin" };
    RegisterClass(&wc);
    HWND win = CreateWindow("GLWin", "Green Square", WS_OVERLAPPEDWINDOW, 0, 0, 800, 600, 0, 0, wc.hInstance, 0);
    ShowWindow(win, SW_SHOW);

    HDC dc = GetDC(win);
    PIXELFORMATDESCRIPTOR pfd = { sizeof(pfd), 1, PFD_DRAW_TO_WINDOW | PFD_SUPPORT_OPENGL | PFD_DOUBLEBUFFER, PFD_TYPE_RGBA };
    SetPixelFormat(dc, ChoosePixelFormat(dc, &pfd), &pfd);
    HGLRC rc = wglCreateContext(dc);
    wglMakeCurrent(dc, rc);

    glMatrixMode(GL_PROJECTION); glLoadIdentity();
    glOrtho(-1, 1, -1, 1, -1, 1);
    glMatrixMode(GL_MODELVIEW);

    MSG msg;
    while (!PeekMessage(&msg, NULL, 0, 0, PM_REMOVE) || msg.message != WM_QUIT) {
        if (msg.message) { TranslateMessage(&msg); DispatchMessage(&msg); }

        glClearColor(0, 0, 0, 1);
        glClear(GL_COLOR_BUFFER_BIT);
        glColor3f(0, 1, 0);
        glBegin(GL_QUADS);
        glVertex2f(-0.5f, -0.5f); glVertex2f(0.5f, -0.5f);
        glVertex2f(0.5f, 0.5f);  glVertex2f(-0.5f, 0.5f);
        glEnd();
        SwapBuffers(dc);
        Sleep(16);
    }

    wglMakeCurrent(NULL, NULL);
    wglDeleteContext(rc);
    ReleaseDC(win, dc);
    DestroyWindow(win);
    return 0;
}
```
