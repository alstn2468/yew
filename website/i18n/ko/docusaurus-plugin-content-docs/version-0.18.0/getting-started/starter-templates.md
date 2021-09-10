---
title: "스타터 템플릿"
---

## `trunk`

- [Minimal Template](https://github.com/yewstack/yew-trunk-minimal-template)은 시작할 수 있도록 Trunk로 구축된 작은 do플리케이션 입니다.

## `wasm-pack`

- [Minimal Template](https://github.com/yewstack/yew-wasm-pack-minimal)은 `wasm-pack`과
  `rollup`을 사용하여 do플리케이션 및 이를 서비스할 자체 서버를 구축합니다. 더 추가할 것은 없습니다.

- [Webpack Template](https://github.com/yewstack/yew-wasm-pack-template)은 개발을 간소화 하기 위해 Webpack으로 `wasm-pack` 과
  [`wasm-pack-plugin`](https://github.com/wasm-tool/wasm-pack-plugin)을 사용합니다.


다른 도구들과는 다르게 `wasm-pack`은 `bin` crate가 아닌 `lib` crate를 사용하도록 강제합니다.
그리고 프로그램의 엔트리 포인트는 `#[wasm_bindgen(start)]`속성과 함께 명시되어 있습니다.

`Cargo.toml` 또한 crate의 타입이 "cdylib"인지 명시해야 합니다.

```toml
[package]
name = "yew-app"
version = "0.1.0"
authors = ["Yew App Developer <name@example.com>"]
edition = "2018"

[lib]
# "rlib"(crate의 기본 타입)을 포함하여야만 crate가 Rust library로 사용될 수 있습니다.
# 다른 무엇보다도, 유닛 테스트를 위반합니다.
crate-type = ["rlib", "cdylib"]

[dependencies]
# for web_sys
yew = "0.17"
# or for stdweb
# yew = { version = "0.17", package = "yew-stdweb" }
wasm-bindgen = "0.2"
```

## Other templates

- [Parcel Template](https://github.com/spielrs/yew-parcel-template)은 커뮤니티 구성원에 의해 작성되었으며, 
  [Parcel](https://parceljs.org/)을 사용합니다.
