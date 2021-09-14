---
title: "cargo-web사용"
---

Cargo web은 클라이언트 웹 애플리케이션을 구축하기 위한 cargo 서브 명령어입니다. 
이를 통해서 빌드와 배포를 매우 쉽게 할 수 있습니다.  
또한 동시에 Emscripten가 타겟인 것을 유일하게 지원하는 툴 체인입니다.

더보기 [여기](https://github.com/koute/cargo-web).

**설치**

```bash
cargo install cargo-web
```

## 빌드

```bash
cargo web build
```

## 실행

```bash
cargo web start
```

## 지원되는 대상

* `wasm32-unknown-unknown`
* `wasm32-unknown-emscripten`
* `asmjs-unknown-emscripten`

:::주의
 `*-emscripten` 을 타겟으로 하는 경우, Emscripten SDK를 설치 해야 한다.
:::
