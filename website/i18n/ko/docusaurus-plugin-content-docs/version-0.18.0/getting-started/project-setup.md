---
title: "프로젝트 셋업"
sidebar_label: 개요
description: "성공을 향한 준비 작업"
---

## Rust

우선, Rust가 설치되어있어야 합니다. Rust와 `cargo` 빌드 도구 설치를 위해서는 [공식 문서](https://www.rust-lang.org/tools/install)의 지시사항을 따르면 됩니다.

Rust를 Wasm으로 컴파일하기 위해 `wasm32-unknown-unknown` 타겟도 설치해야 합니다.
rustup을 사용 중이라면 `rustup target add wasm32-unknown-unknown`을 실행하기만 하면 됩니다.

:::중요
Yew가 지원하는 가장 낮은 Rust 버전(MSRV)은 `1.49.0`입니다. 그 이전 버전을 사용하면 예상 못한 문제와 이해할 수 없는 에러 메시지를 경험할 수 있습니다.
툴 체인 버전을 확인하려면 "active toolchain"에서 `rustup show` 혹은 `rustc --version`를 실행하면 됩니다. 툴 체인을 업데이트하려면 `rustup update`를 실행하세요.
:::

## **Wasm 빌드 도구**

WebAssembly와 JavaScript를 함께 사용하기 위해서는 별도의 도구를 추가해야 합니다.
어떤 도구를 선택했는지에 따라 개발과 패키징 작업이 훨씬 간편해질 수 있습니다.
어플리케이션의 `.wasm` 바이너리를 브라우저에 로드하고 실행하는 데 필요한 모든 JavaScript 코드를 생성해주는 기능까지 제공됩니다.

### [**`trunk`**](https://github.com/thedodd/trunk/)

Yew 어플리케이션 빌드를 위해 만들어진 도구입니다.
모든 `wasm-bindgen` 기반 어플리케이션을 빌드할 수 있으며, rollup.js의 구조에 영향을 받았습니다.
Trunk를 사용하면 빌드를 위해 Node.js를 설치하거나 JavaScript 코드를 작성할 필요가 완전히 사라집니다.
어플리케이션을 위한 에셋들을 번들링해주면서 Sass 컴파일러까지 제공됩니다.

해당 문서의 모든 예시 프로젝트들도 Trunk를 사용하고 있습니다.

[`trunk`로 프로젝트 시작하기](project-setup/using-trunk.md)

### [**`wasm-pack`**](https://rustwasm.github.io/docs/wasm-pack/)

WebAssembly 패키징을 위해 Rust / Wasm Working Group에서 개발한 CLI 도구입니다.
웹팩을 위해 [`wasm-pack-plugin`](https://github.com/wasm-tool/wasm-pack-plugin)을 함께 사용하는 것이 가장 권장됩니다.
`wasm-pack`은 주로 JavaScript에서 사용할 수 있는 Wasm 라이브러리를 만들기 위해 사용됩니다.
때문에 라이브러리들만 빌드할 수 있으며, 개발 서버나 자동 빌드 기능과 같은 도구를 제공해주지 않습니다.

[`wasm-pack`으로 프로젝트 시작하기](project-setup/using-wasm-pack.md)

### [**`cargo-web`**](https://github.com/koute/cargo-web)

`wasm-bindgen`이 등장하기 전까지 가장 선호되는 빌드 도구였습니다.

[`cargo web`으로 프로젝트 시작하기](project-setup/using-cargo-web.md)

### Comparison

|                           | `trunk`                                                    | `wasm-pack`                                                                                            | `cargo-web`                                                                                                                                            |
| ------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 프로젝트 현황             | 활발하게 유지보수 중                                       | [Rust / Wasm Working Group](https://rustwasm.github.io)에서 활발하게 유지보수 중                       | 6개월 넘게 아무런 깃헙 활동이 없음                                                                                                                     |
| 개발 경험                 | 별도의 외부 라이브러리 없이 그 자체만으로 동작합니다.      | 뼈대만 제공됩니다. 별도의 스크립트를 짜거나, 웹팩 플러그인을 사용해야 개발경험을 향상시킬 수 있습니다. | 코딩할 때는 편하지만 별도의 에셋 파이프라인을 필요로 합니다.                                                                                           |
| 로컬 서버                 | 지원됨                                                     | 웹팩 플러그인이 있어야 지원됨                                                                          | 지원됨                                                                                                                                                 |
| 변경상황에 대한 자동 빌드 | 지원됨                                                     | 웹팩 플러그인이 있어야 지원됨                                                                          | 지원됨                                                                                                                                                 |
| 에셋 관리                 | 지원됨                                                     | 웹팩 플러그인이 있어야 지원됨                                                                          | 정적 에셋만 지원됨                                                                                                                                     |
| 헤드리스 브라우저 테스팅  | [개발 진행 중](https://github.com/thedodd/trunk/issues/20) | [지원됨](https://rustwasm.github.io/wasm-pack/book/commands/test.html)                                 | [지원됨](https://github.com/koute/cargo-web#features)                                                                                                  |
| 지원되는 타겟             | <ul><li><code>wasm32-unknown-unknown</code></li></ul>      | <ul><li><code>wasm32-unknown-unknown</code></li></ul>                                                  | <ul> <li><code>wasm32-unknown-unknown</code></li> <li><code>wasm32-unknown-emscripten</code></li> <li><code>asmjs-unknown-emscripten</code></li> </ul> |
| `web-sys`                 | 호환됨                                                     | 호환됨                                                                                                 | 호환되지 않음                                                                                                                                          |
| `stdweb`                  | 호환되지 않음                                              | 호환됨                                                                                                 | 호환됨                                                                                                                                                 |
| 사용 예시                 | [샘플 어플리케이션](./build-a-sample-app.md)               | [스타터 템플릿](https://github.com/yewstack/yew-wasm-pack-minimal)                                     | `yew-stdweb` 예시를 위한 [빌드 스크립트](https://www.github.com/yewstack/yew/tree/master/packages/yew-stdweb/examples)                                 |
