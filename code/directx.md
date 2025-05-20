# 🔹 Direct3D란?

* **DirectX의 일부**로, DirectX는 멀티미디어 작업(그래픽, 사운드 등)을 위한 API 모음입니다.
* **Windows 플랫폼 전용**이며, 하드웨어 가속을 통해 고성능 그래픽을 구현할 수 있게 해줍니다.
* \*\*GPU(Graphics Processing Unit)\*\*를 제어하여 **3D 모델 렌더링**, **텍스처 처리**, **셰이더 프로그래밍** 등을 수행합니다.

---

### 🔹 주요 특징

| 항목         | 설명                                      |
| ---------- | --------------------------------------- |
| **플랫폼**    | Windows 전용 (Xbox 포함)                    |
| **그래픽 타입** | 주로 3D (2D도 가능)                          |
| **셰이더 지원** | Vertex, Pixel, Geometry 등 다양한 셰이더       |
| **버전**     | Direct3D 9, 10, 11, 12 (버전별로 성능과 기능 향상) |
| **API 타입** | 로우레벨 API (OpenGL과 유사, Vulkan보다는 덜 저수준)  |

---

### 🔹 기본 렌더링 파이프라인 (간단 예시)

1. **Vertex Buffer 생성** – 정점 데이터를 준비
2. **셰이더 컴파일** – Vertex/Pixel 셰이더 작성 및 적용
3. **렌더 타겟 설정** – 그릴 화면을 준비
4. **Draw 호출** – 실제로 그리기 명령을 GPU에 전달
5. **Present** – 결과를 화면에 출력

---

### 🔹 Direct3D vs 기타 API

| 항목    | Direct3D     | OpenGL | Vulkan       |
| ----- | ------------ | ------ | ------------ |
| 플랫폼   | Windows/Xbox | 크로스플랫폼 | 크로스플랫폼       |
| 난이도   | 중간           | 중간     | 높음 (복잡)      |
| 성능 제어 | 자동 최적화 많음    | 제한적    | 매우 세밀한 제어 가능 |

---

# 초록색 바탕
```
#include <windows.h>
#include <d3d11.h>
#pragma comment(lib, "d3d11.lib")

IDXGISwapChain* sc;
ID3D11Device* dev;
ID3D11DeviceContext* ctx;
ID3D11RenderTargetView* rtv;

LRESULT CALLBACK WndProc(HWND h, UINT m, WPARAM w, LPARAM l) {
    if (m == WM_DESTROY) PostQuitMessage(0);
    return DefWindowProc(h, m, w, l);
}

int WINAPI WinMain(HINSTANCE h, HINSTANCE, LPSTR, int) {
    WNDCLASS wc = { CS_OWNDC, WndProc, 0, 0, h, 0, 0, 0, 0, L"D3D" };
    RegisterClass(&wc);
    HWND win = CreateWindow(L"D3D", 0, WS_OVERLAPPEDWINDOW | WS_VISIBLE, 100, 100, 800, 600, 0, 0, h, 0);

    DXGI_SWAP_CHAIN_DESC sd = {};
    sd.BufferCount = 1;
    sd.BufferDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
    sd.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
    sd.OutputWindow = win;
    sd.SampleDesc.Count = 1;
    sd.Windowed = TRUE;

    D3D11CreateDeviceAndSwapChain(0, D3D_DRIVER_TYPE_HARDWARE, 0, 0, 0, 0, D3D11_SDK_VERSION,
        &sd, &sc, &dev, 0, &ctx);

    ID3D11Texture2D* bb;
    sc->GetBuffer(0, __uuidof(ID3D11Texture2D), (void**)&bb);
    dev->CreateRenderTargetView(bb, 0, &rtv);
    bb->Release();
    ctx->OMSetRenderTargets(1, &rtv, 0);

    MSG msg = {};
    while (msg.message != WM_QUIT) {
        if (PeekMessage(&msg, 0, 0, 0, PM_REMOVE)) DispatchMessage(&msg);
        float green[4] = { 0, 1, 0, 1 };
        ctx->ClearRenderTargetView(rtv, green);
        sc->Present(1, 0);
    }

    rtv->Release(); sc->Release(); ctx->Release(); dev->Release();
    return 0;
}
```
# wireframe
```
#include "pch.h"
#include <windows.h>
#include <d3d11.h>
#include <d3dcompiler.h>
#include <DirectXMath.h>
#pragma comment(lib, "d3d11.lib")
#pragma comment(lib, "d3dcompiler.lib")

using namespace DirectX;

HWND hwnd;
ID3D11Device* dev;
ID3D11DeviceContext* ctx;
IDXGISwapChain* sc;
ID3D11RenderTargetView* rtv;
ID3D11VertexShader* vs;
ID3D11PixelShader* ps;
ID3D11InputLayout* layout;
ID3D11Buffer* vb, * ib, * cb;
ID3D11RasterizerState* wireRS;

struct Vtx { XMFLOAT3 pos, col; };
struct CB { XMMATRIX mvp; };

LRESULT CALLBACK WndProc(HWND h, UINT m, WPARAM w, LPARAM l) {
    if (m == WM_DESTROY) PostQuitMessage(0);
    return DefWindowProc(h, m, w, l);
}

void InitD3D() {
    DXGI_SWAP_CHAIN_DESC d = {};
    d.BufferDesc.Width = 800; d.BufferDesc.Height = 600;
    d.BufferDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
    d.SampleDesc.Count = 1; d.BufferCount = 1;
    d.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
    d.OutputWindow = hwnd; d.Windowed = TRUE;
    D3D11CreateDeviceAndSwapChain(0, D3D_DRIVER_TYPE_HARDWARE, 0, 0, 0, 0,
        D3D11_SDK_VERSION, &d, &sc, &dev, 0, &ctx);

    ID3D11Texture2D* buf;
    sc->GetBuffer(0, __uuidof(ID3D11Texture2D), (void**)&buf);
    dev->CreateRenderTargetView(buf, 0, &rtv); buf->Release();

    const char* vsCode =
        "cbuffer cb : register(b0) {matrix mvp;} "
        "struct In {float3 p:POSITION; float3 c:COLOR;};"
        "struct Out {float4 p:SV_POSITION; float3 c:COLOR;};"
        "Out main(In i) {Out o; o.p = mul(float4(i.p,1),mvp); o.c=i.c; return o;}";

    const char* psCode =
        "struct In {float4 p:SV_POSITION; float3 c:COLOR;};"
        "float4 main(In i) : SV_Target { return float4(i.c,1); }";

    ID3DBlob* vsb, * psb;
    D3DCompile(vsCode, strlen(vsCode), 0, 0, 0, "main", "vs_5_0", 0, 0, &vsb, 0);
    D3DCompile(psCode, strlen(psCode), 0, 0, 0, "main", "ps_5_0", 0, 0, &psb, 0);
    dev->CreateVertexShader(vsb->GetBufferPointer(), vsb->GetBufferSize(), 0, &vs);
    dev->CreatePixelShader(psb->GetBufferPointer(), psb->GetBufferSize(), 0, &ps);

    D3D11_INPUT_ELEMENT_DESC ied[] = {
        {"POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0},
        {"COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0,12, D3D11_INPUT_PER_VERTEX_DATA, 0},
    };
    dev->CreateInputLayout(ied, 2, vsb->GetBufferPointer(), vsb->GetBufferSize(), &layout);
    vsb->Release(); psb->Release();

    Vtx v[] = {
        {{-1,-1,-1},{1,0,0}}, {{-1,+1,-1},{0,1,0}}, {{+1,+1,-1},{0,0,1}}, {{+1,-1,-1},{1,1,0}},
        {{-1,-1,+1},{1,0,1}}, {{-1,+1,+1},{0,1,1}}, {{+1,+1,+1},{1,1,1}}, {{+1,-1,+1},{0,0,0}},
    };
    DWORD idx[] = {
        0,1,1,2,2,3,3,0, 4,5,5,6,6,7,7,4, 0,4,1,5,2,6,3,7,
    };

    D3D11_BUFFER_DESC bd = { sizeof(v), D3D11_USAGE_DEFAULT, D3D11_BIND_VERTEX_BUFFER };
    D3D11_SUBRESOURCE_DATA s = { v }; dev->CreateBuffer(&bd, &s, &vb);
    bd.ByteWidth = sizeof(idx); bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    s.pSysMem = idx; dev->CreateBuffer(&bd, &s, &ib);
    bd.ByteWidth = sizeof(CB); bd.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
    dev->CreateBuffer(&bd, 0, &cb);

    D3D11_RASTERIZER_DESC rs = {};
    rs.FillMode = D3D11_FILL_WIREFRAME; rs.CullMode = D3D11_CULL_NONE;
    dev->CreateRasterizerState(&rs, &wireRS);

    D3D11_VIEWPORT vp = { 0, 0, 800, 600, 0, 1 };
    ctx->RSSetViewports(1, &vp);
}

void Render(float angle) {
    float bg[4] = { 0.05f,0.05f,0.1f,1 }; ctx->ClearRenderTargetView(rtv, bg);
    ctx->OMSetRenderTargets(1, &rtv, 0);

    XMMATRIX w = XMMatrixRotationY(angle);
    XMMATRIX v = XMMatrixLookAtLH({ 0,0,-5 }, { 0,0,0 }, { 0,1,0 });
    XMMATRIX p = XMMatrixPerspectiveFovLH(XM_PIDIV4, 800.0f / 600.0f, 0.1f, 100);
    XMMATRIX mvp = XMMatrixTranspose(w * v * p);
    ctx->UpdateSubresource(cb, 0, 0, &mvp, 0, 0);

    UINT stride = sizeof(Vtx), offset = 0;
    ctx->IASetInputLayout(layout);
    ctx->IASetVertexBuffers(0, 1, &vb, &stride, &offset);
    ctx->IASetIndexBuffer(ib, DXGI_FORMAT_R32_UINT, 0);
    ctx->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_LINELIST);
    ctx->RSSetState(wireRS);
    ctx->VSSetShader(vs, 0, 0); ctx->VSSetConstantBuffers(0, 1, &cb);
    ctx->PSSetShader(ps, 0, 0);
    ctx->DrawIndexed(24, 0, 0);
    sc->Present(1, 0);
}

int WINAPI WinMain(HINSTANCE h, HINSTANCE, LPSTR, int) {
    WNDCLASS wc = { CS_OWNDC, WndProc, 0,0,h, 0,0,0,0,L"D3D" };
    RegisterClass(&wc);
    hwnd = CreateWindow(L"D3D", L"WireCube", WS_OVERLAPPEDWINDOW, 100, 100, 800, 600, 0, 0, h, 0);
    ShowWindow(hwnd, SW_SHOW); InitD3D();

    MSG msg = {}; float a = 0;
    while (msg.message != WM_QUIT) {
        if (PeekMessage(&msg, 0, 0, 0, PM_REMOVE)) DispatchMessage(&msg);
        else { a += 0.01f; Render(a); }
    }
    return 0;
}

```
# 무지게 삼각형
```
#include "pch.h"
#include <windows.h>
#include <d3d11.h>
#include <d3dcompiler.h>
#pragma comment(lib, "d3d11.lib")
#pragma comment(lib, "d3dcompiler.lib")

IDXGISwapChain* sc;
ID3D11Device* dev;
ID3D11DeviceContext* ctx;
ID3D11RenderTargetView* rtv;

ID3D11VertexShader* vs;
ID3D11PixelShader* ps;
ID3D11InputLayout* layout;
ID3D11Buffer* vb;

LRESULT CALLBACK WndProc(HWND h, UINT m, WPARAM w, LPARAM l) {
    if (m == WM_DESTROY) PostQuitMessage(0);
    return DefWindowProc(h, m, w, l);
}

// HLSL 셰이더 코드
const char* vshader =
"struct VS_IN { float3 pos : POSITION; float3 col : COLOR; }; \
 struct VS_OUT { float4 pos : SV_POSITION; float3 col : COLOR; }; \
 VS_OUT main(VS_IN input) { VS_OUT o; o.pos = float4(input.pos, 1); o.col = input.col; return o; }";

const char* pshader =
"struct VS_OUT { float4 pos : SV_POSITION; float3 col : COLOR; }; \
 float4 main(VS_OUT input) : SV_TARGET { return float4(input.col, 1); }";

int WINAPI WinMain(HINSTANCE h, HINSTANCE, LPSTR, int) {
    // 윈도우 생성
    WNDCLASS wc = { CS_OWNDC, WndProc, 0, 0, h, 0, 0, 0, 0, L"D3D" };
    RegisterClass(&wc);
    HWND win = CreateWindow(L"D3D", 0, WS_OVERLAPPEDWINDOW | WS_VISIBLE,
        100, 100, 800, 600, 0, 0, h, 0);

    // DX11 초기화
    DXGI_SWAP_CHAIN_DESC sd = {};
    sd.BufferCount = 1;
    sd.BufferDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
    sd.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
    sd.OutputWindow = win;
    sd.SampleDesc.Count = 1;
    sd.Windowed = TRUE;

    D3D11CreateDeviceAndSwapChain(0, D3D_DRIVER_TYPE_HARDWARE, 0, 0, 0, 0,
        D3D11_SDK_VERSION, &sd, &sc, &dev, 0, &ctx);

    ID3D11Texture2D* bb;
    sc->GetBuffer(0, __uuidof(ID3D11Texture2D), (void**)&bb);
    dev->CreateRenderTargetView(bb, 0, &rtv);
    bb->Release();
    ctx->OMSetRenderTargets(1, &rtv, 0);

    // ✅ Viewport 설정
    D3D11_VIEWPORT vp = {};
    vp.TopLeftX = 0;
    vp.TopLeftY = 0;
    vp.Width = 800;
    vp.Height = 600;
    vp.MinDepth = 0.0f;
    vp.MaxDepth = 1.0f;
    ctx->RSSetViewports(1, &vp);

    // 셰이더 컴파일
    ID3DBlob* vsBlob, * psBlob;
    D3DCompile(vshader, strlen(vshader), 0, 0, 0, "main", "vs_4_0", 0, 0, &vsBlob, 0);
    D3DCompile(pshader, strlen(pshader), 0, 0, 0, "main", "ps_4_0", 0, 0, &psBlob, 0);

    dev->CreateVertexShader(vsBlob->GetBufferPointer(), vsBlob->GetBufferSize(), 0, &vs);
    dev->CreatePixelShader(psBlob->GetBufferPointer(), psBlob->GetBufferSize(), 0, &ps);
    ctx->VSSetShader(vs, 0, 0);
    ctx->PSSetShader(ps, 0, 0);

    // 정점 입력 레이아웃
    D3D11_INPUT_ELEMENT_DESC ied[] = {
        { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
        { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    };
    dev->CreateInputLayout(ied, 2, vsBlob->GetBufferPointer(), vsBlob->GetBufferSize(), &layout);
    ctx->IASetInputLayout(layout);

    // 정점 버퍼 생성 (무지개 색 삼각형)
    struct VERTEX { float x, y, z; float r, g, b; };
    VERTEX v[] = {
        {  0.0f,  0.5f, 0.0f, 1, 0, 0 }, // 빨강 (상단)
        {  0.5f, -0.5f, 0.0f, 0, 1, 0 }, // 초록 (우하)
        { -0.5f, -0.5f, 0.0f, 0, 0, 1 }  // 파랑 (좌하)
    };
    D3D11_BUFFER_DESC bd = { sizeof(v), D3D11_USAGE_DEFAULT, D3D11_BIND_VERTEX_BUFFER, 0, 0, 0 };
    D3D11_SUBRESOURCE_DATA sdv = { v };
    dev->CreateBuffer(&bd, &sdv, &vb);

    ctx->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    UINT stride = sizeof(VERTEX), offset = 0;
    ctx->IASetVertexBuffers(0, 1, &vb, &stride, &offset);

    // 렌더링 루프
    MSG msg = {};
    while (msg.message != WM_QUIT) {
        if (PeekMessage(&msg, 0, 0, 0, PM_REMOVE)) DispatchMessage(&msg);

        float clearColor[4] = { 0.1f, 0.1f, 0.1f, 1 };
        ctx->ClearRenderTargetView(rtv, clearColor);
        ctx->Draw(3, 0);
        sc->Present(1, 0);
    }

    // 자원 해제
    vb->Release();
    layout->Release();
    vs->Release();
    ps->Release();
    rtv->Release();
    sc->Release();
    ctx->Release();
    dev->Release();

    return 0;
}
```
