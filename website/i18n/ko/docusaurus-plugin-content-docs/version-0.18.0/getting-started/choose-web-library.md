---
title: "웹 라이브러리 선택하기"
---

## 개요

Yew 애플리케이션은 [`web-sys`](https://docs.rs/web-sys) 혹은 [`stdweb`](https://docs.rs/stdweb)을 사용하여 개발할 수 있습니다. 두 crate 모두 Rust와 Web API 사이를 이어주는 역할을 합니다. `yew` 를 cargo 의존성 목록에 추가할 때 두 가지 중 하나를 선택해야 합니다.

```toml
# `web-sys`를 선택한 경우
yew = "0.17"

# `stdweb`을 선택한 경우
yew = { version = "0.17", package = "yew-stdweb" }
```

[Rust / Wasm 활동 집단](https://rustwasm.github.io/)가 꾸준히 작업 중이라는 점에서 `web-sys`를 사용하는 것을 권장합니다.

:::주의
Yew는 v0.18부터 `stdweb`에 대한 지원을 중단할 것입니다. 버그는 고쳐질 것이지만 새로운 기능이 추가되지는 않을 것입니다. 자세한 사항은 [#1569](https://github.com/yewstack/yew/issues/1569)을 참고하세요.
:::

## 사용 예시

해당 예시는 두 라이브러리를 사용하는 방식에 어떠한 차이가 있는지를 보여줍니다. 직접 이 코드를 실행할 필요는 없습니다.

```rust
// web-sys
let window: web_sys::Window = web_sys::window().expect("window not available");
window.alert_with_message("hello from wasm!").expect("alert failed");

// stdweb
let window: stdweb::web::Window = stdweb::web::window();
window.alert("hello from wasm!");

// js! 매크로를 활용한 stdweb
use stdweb::js;
use stdweb::unstable::TryFrom;
use stdweb::web::Window;

let window_val: stdweb::Value = js!{ return window; }; // <- JavaScript 문법 포함!
let window = Window::try_from(window_val).expect("conversion to window failed");
window.alert("hello from wasm!");
```

각 crate의 API는 살짝 다르지만 전반적으로 동일한 기능을 수행합니다.

## 선택하기

`web-sys`와 `stdweb` 중 하나를 선택하기 위해 다각도에서 생각해볼 수 있다. 물론 하나의 앱에서 둘 다 사용하는 것도 가능하다. 하지만 컴파일된 crate의 바이너리 크기를 최소화하기 위해서는 둘 중 한 가지만 사용하는 것이 최선이다.

<table>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}></th>
      <th style={{ textAlign: "left" }}><code>web-sys</code>
      </th>
      <th style={{ textAlign: "left" }}><code>stdweb</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>프로젝트 현황</td>
      <td style={{ textAlign: "left" }}><a href="https://rustwasm.github.io/">Rust / Wasm 활동 집단</a>에 의해 활발하게 유지보수 중인 상황</td>
      <td style={{ textAlign: "left" }}>8개월 넘게 아무런 깃헙 활동이 없는 상황</td>
    </tr>
    <tr>
      <td style={{ textAlign: "left" }}>Web API 지원 범위</td>
      <td style={{ textAlign: "left" }}>Rust API는 Web IDL 사양을 기반으로 생성됩니다.</td>
      <td style={{ textAlign: "left" }}>필요에 따라 커뮤니티에 의해 브라우저 API가 추가됩니다.</td>
    </tr>
    <tr>
      <td style={{ textAlign: "left" }}>Rust API 구조</td>
      <td style={{ textAlign: "left" }}>대부분의 API 요청에 대해 <code>Result</code>를 반환함으로써 보수적으로 접근합니다.</td>
      <td style={{ textAlign: "left" }}>문제 발생시 대체로 <code>Result</code> 대신 panic을 반환하고자 합니다. 예를 들어 <code>stdweb::web::window()</code>가 worker 내부에서 호출되면 panic이 발생합니다.</td>
    </tr>
    <tr>
      <td style={{ textAlign: "left" }}>지원되는 빌드 도구</td>
      <td style={{ textAlign: "left" }}>
        <p></p>
        <ul>
          <li><code>trunk</code>
          </li>
          <li><code>wasm-pack</code>
          </li>
        </ul>
      </td>
      <td style={{ textAlign: "left" }}>
        <p></p>
        <ul>
          <li><code>cargo-web</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style={{ textAlign: "left" }}>지원 대상</td>
      <td style={{ textAlign: "left" }}>
        <ul>
          <li><code>wasm32-unknown-unknown</code>
          </li>
        </ul>
      </td>
      <td style={{ textAlign: "left" }}>
        <ul>
          <li><code>wasm32-unknown-unknown</code>
          </li>
          <li><code>wasm32-unknown-emscripten</code>
          </li>
          <li><code>asmjs-unknown-emscripten</code>
          </li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>
