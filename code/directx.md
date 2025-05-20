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
