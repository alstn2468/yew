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

Rust 환경이 셋업된 것을 확인하려면 cargo 빌드 도구를 이용하여 초기 프로젝트를 실행하면 됩니다.
빌드 과정에 대한 내용 출력이후 "Hello World"라는 메세지를 확인 할 수 있을겁니다.


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

`Model`이라는 루트 `컴포넌트`를 설정하여며, 클릭하면 자동으로 업데이트되는 버튼을 표시하는 탬플릿을 만들어야 합니다.
`src/main.rs`의 내용을 다음과 같이 수정합니다.

:::note
`main()`안의 `yew::start_app::<Model>()`은 애플리케이션을 실행시키고 페이지의 `<body>`태그에 적용시킵니다.  
만약 애플리케이션을 동적속성과 함께 실행하고 싶다면, 아래와 같이 작성하면 됩니다.
`yew::start_app_with_props::<Model>(..)`.
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
                 // value가 업데이트되었으므로
                 // re-render 하여 페이지에 보이도록 합니다.
                true
            }
        }
    }

    fn view(&self, ctx: &Context<Self>) -> Html {
        // 이는 컴포넌트의 "`Scope`"를 제공하며, 이는 컴포넌트로 메시지를 보내는 용도등으로 사용할 수 있습니다. 
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

애플리케이션을 다음 명령어를 실행한다면 로컬 환경에서 빌드가 되며 실행될 것 입니다.

```bash
trunk serve
```

Trunk의 파일이 수정된다면 Trunk는 애플리케이션을 다시 빌드 할것입니다.

## 축하합니다

이제 Yew 개발 환경을 성공적으로 셋업하였고 첫 번째 웹 애플리케이션을 빌드했습니다.

이 애플리케이션을 실험해보고 [예시](./examples.md)를 통해 더 학습해 보세요.