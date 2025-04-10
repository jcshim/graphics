### ✅ WebGPU의 핵심 특징

1. **GPU-Native 접근**
   - Vulkan, Metal, Direct3D 12 등의 로우레벨 GPU API 위에서 동작
   - WebGL보다 낮은 오버헤드, 높은 성능 제공

2. **그래픽 + 컴퓨팅 통합**
   - 렌더링뿐 아니라 **병렬 계산(GPGPU)**도 지원
   - 예: 머신러닝, 시뮬레이션, 벡터 검색 등

3. **WGSL (WebGPU Shading Language) 사용**
   - GLSL이 아닌 자체 셰이더 언어(WGSL)를 사용
   - 안전성과 이식성이 뛰어남

4. **비동기 리소스 로딩, 파이프라인 캐싱 등 고급 기능**

---

### 🎮 주요 활용 분야

- **고성능 게임 렌더링**
- **실시간 데이터 시각화** (3D 시각화, 과학 계산)
- **머신러닝 Inference** (브라우저 내 실행)
- **GPU 기반 벡터 검색**
- **웹 기반 CAD, PCB 설계 도구**

---

### 💻 예제: WebGPU로 삼각형 출력

```javascript
// WebGPU context 얻기
const canvas = document.querySelector("canvas");
const adapter = await navigator.gpu.requestAdapter();
const device = await adapter.requestDevice();
const context = canvas.getContext("webgpu");

context.configure({
  device,
  format: navigator.gpu.getPreferredCanvasFormat(),
});

// 셰이더 코드 (WGSL)
const shaderCode = `
  @vertex
  fn vs_main(@builtin(vertex_index) i: u32) -> @builtin(position) vec4f {
    var pos = array<vec2f, 3>(
      vec2f(0.0, 0.5),
      vec2f(-0.5, -0.5),
      vec2f(0.5, -0.5)
    );
    return vec4f(pos[i], 0.0, 1.0);
  }

  @fragment
  fn fs_main() -> @location(0) vec4f {
    return vec4f(0.0, 1.0, 0.0, 1.0); // 녹색
  }
`;

// 셰이더 모듈 생성
const shaderModule = device.createShaderModule({ code: shaderCode });

// 파이프라인 설정
const pipeline = device.createRenderPipeline({
  layout: "auto",
  vertex: {
    module: shaderModule,
    entryPoint: "vs_main",
  },
  fragment: {
    module: shaderModule,
    entryPoint: "fs_main",
    targets: [{ format: navigator.gpu.getPreferredCanvasFormat() }],
  },
  primitive: { topology: "triangle-list" },
});

// 렌더 패스
const encoder = device.createCommandEncoder();
const textureView = context.getCurrentTexture().createView();

const renderPass = encoder.beginRenderPass({
  colorAttachments: [{
    view: textureView,
    clearValue: { r: 0, g: 0, b: 0, a: 1 },
    loadOp: 'clear',
    storeOp: 'store',
  }]
});

renderPass.setPipeline(pipeline);
renderPass.draw(3);
renderPass.end();

device.queue.submit([encoder.finish()]);
```

---

### 📦 WebGPU 사용 준비

- **지원 브라우저**: Chromium 기반 브라우저 (Chrome, Edge 등), Safari 일부
- **실험 플래그 활성화 필요할 수 있음**
  - Chrome: `chrome://flags/#enable-unsafe-webgpu`

---

### 📚 참고 자료

- [WebGPU 공식 문서](https://gpuweb.github.io/gpuweb/)
- [WebGPU Samples by Google](https://github.com/austinEng/webgpu-samples)
- [WGSL 언어 사양](https://www.w3.org/TR/WGSL/)

---

WebGPU로 뭘 만들고 싶으신가요? 게임, 시각화, 머신러닝, 아니면 직접 셰이더를 구현해보고 싶으신가요? 원하시면 예제나 튜토리얼 더 준비해드릴게요!
