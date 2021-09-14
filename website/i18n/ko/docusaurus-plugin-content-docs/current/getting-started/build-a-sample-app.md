---
title: "간단한 앱 구축하기"
---

## Project 생성하기

새롭게 생성된 폴더를 엽니다.

```bash
cargo new yew-app
```

새롭게 생성된 폴더를 엽니다.

```bash
cd yew-app
```

## hello world 예시 실행하기

To verify the Rust environment is setup, run the initial project using the cargo build tool.  After output about the build process, you should see the expected "Hello World" message.


```bash
cargo run
```

## 프로젝트를 Yew 웹 애플리케이션으로 변경하기

이 간단한 명령 행 애플리케이션을 기본 Yew 웹 애플리케이션으로 변경하려면 약간의 수정이 필요합니다.

### Cargo.toml 수정하기

`Cargo.toml`파일에 'yew'를 추가합니다:

```toml
[package]
name = "yew-app"
version = "0.1.0"
edition = "2018"

[dependencies]
# you can check the latest version here: https://crates.io/crates/yew
yew = "0.17"
```

### main.rs 수정하기

We need to generate a template which sets up a root Component called `Model` which renders a button that updates its value when clicked.
Replace the contents of `src/main.rs` with the following code.

:::note
The line `yew::start_app::<Model>()` inside `main()` starts your application and mounts it to the page's `<body>` tag.  
If you would like to start your application with any dynamic properties, you can instead use `yew::start_app_with_props::<Model>(..)`.
:::


```rust ,no_run
use yew::prelude::*;

enum Msg {
    AddOne,
}

struct Model {
    value: i64,
}

impl Component for Model {
    type Message = Msg;
    type Properties = ();

    fn create(_ctx: &Context<Self>) -> Self {
        Self {
            value: 0,
        }
    }

    fn update(&mut self, _ctx: &Context<Self>, msg: Self::Message) -> bool {
        match msg {
            Msg::AddOne => {
                self.value += 1;
                // the value has changed so we need to
                // re-render for it to appear on the page
                true
            }
        }
    }

    fn view(&self, ctx: &Context<Self>) -> Html {
        // This gives us a component's "`Scope`" which allows us to send messages, etc to the component. 
        let link = ctx.link();
        html! {
            <div>
                <button onclick={link.callback(|_| Msg::AddOne)}>{ "+1" }</button>
                <p>{ self.value }</p>
            </div>
        }
    }
}

fn main() {
    yew::start_app::<Model>();
}
```

### index.html 생성하기

끝으로 애플리케이션의 루트 폴더에 `index.html`파일을 추가합니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Yew App</title>
  </head>
</html>
```

## 웹 애플리케이션 확인하기

Run the following command to build and serve the application locally.

```bash
trunk serve
```

Trunk의 파일이 수정된다면 Trunk는 애플리케이션을 재구축 할것입니다.

## 축하합니다

이제 Yew 개발 환경을 성공적으로 셋업하였고 첫 번째 웹 애플리케이션을 구축했습니다.

Experiment with this application and review the [examples](./examples.md) to further your learning.
이 애플리케이션을 실험해보고 학문을 [예시](./examples.md)을 검토하여 