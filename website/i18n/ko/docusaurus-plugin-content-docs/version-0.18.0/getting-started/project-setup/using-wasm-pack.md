---
title: "wasm-pack 사용하기"
---

이 도구는 WebAssembly 애플리케이션을 구축하기 위해 Rust / Wasm Working Group에서 만들었습니다.코드를 `npm` 모듈로 패키징하는 것을 지원하며 기존 JavaScript 애플리케이션과 쉽게 통합할 수 있도록 함께 제공되는 [Webpack 플러그인](https://github.com/wasm-tool/wasm-pack-plugin)이 있습니다. 자세한 내용은 [`wasm-pack` 문서](https://rustwasm.github.io/docs/wasm-pack/introduction.html)에서 확인할 수 있습니다.

:::참고
`wasm-pack`을 사용하려면 `cdylib`를 포함하도록 명시적으로 crate-type을 설정해야 합니다.

```toml
[lib]
crate-type = ["rlib", "cdylib"]
```

:::

## 설치하기

```bash
cargo install wasm-pack
```

## 빌드하기

이 명령어는 애플리케이션의 컴파일된 WebAssembly와 함께 애플리케이션을 시작하는데 사용할 수 있는 Javascript 래퍼를 `./pkg` 폴더에 번들을 생성합니다.

```bash
wasm-pack build --target web
```

## 번들링하기

rollup.js에 대한 자세한 내용은 이 [가이드](https://rollupjs.org/guide/en/#quick-start)에서 확인할 수 있습니다.

```bash
rollup ./main.js --format iife --file ./pkg/bundle.js
```

rollup.js와 같은 번들러를 사용하는 경우 `--target web`을 생략할 수 있습니다.

## 실행하기

선호하는 서버를 자유롭게 사용할 수 있습니다. 여기서는 간단한 Python 서버를 사용하여 빌드된 앱을 제공합니다.

```bash
python -m http.server 8000
```

Python이 설치되어 있지 않다면 대신 [`simple-http-server`](https://github.com/TheWaWaR/simple-http-server) crate를 설치해 사용할 수 있습니다.
