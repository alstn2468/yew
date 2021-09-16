---
title: "샘플 애플리케이션 구축하기"
---

먼저 새로운 cargo 프로젝트를 생성합니다:

```bash
cargo new yew-app
```

새롭게 생성된 폴더를 엽니다.

먼저 `Cargo.toml`파일에 'yew'를 추가합니다:

```toml
[package]
name = "yew-app"
version = "0.1.0"
edition = "2018"

[dependencies]
# you can check the latest version here: https://crates.io/crates/yew
yew = "0.18"
```

다음 템플릿을 'src/main.rs' 파일에 붙여 넣습니다.


```rust
use yew::prelude::*;

enum Msg {
    AddOne,
}

struct Model {
    // `ComponentLink`는 컴포넌트에 대한 참조와 같습니다.
    // 컴포넌트로 메시지를 보내는 데 사용할 수 있습니다.
    link: ComponentLink<Self>,
    value: i64,
}

impl Component for Model {
    type Message = Msg;
    type Properties = ();

    fn create(_props: Self::Properties, link: ComponentLink<Self>) -> Self {
        Self {
            link,
            value: 0,
        }
    }

    fn update(&mut self, msg: Self::Message) -> ShouldRender {
        match msg {
            Msg::AddOne => {
                self.value += 1;
                // value가 업데이트되었으므로
                // re-render 하여 페이지에 보이도록 합니다.
                true
            }
        }
    }

    fn change(&mut self, _props: Self::Properties) -> ShouldRender {
        // 새로운 속성의 값이 이전 속성값과 다를 때에만 "true"를 리턴해야 합니다.
        // 이 컴포넌트는 속성이 없기 때문에 항상 "false"를 리턴합니다.
        false
    }

    fn view(&self) -> Html {
        html! {
            <div>
                <button onclick=self.link.callback(|_| Msg::AddOne)>{ "+1" }</button>
                <p>{ self.value }</p>
            </div>
        }
    }
}

fn main() {
    yew::start_app::<Model>();
}
```

이 템플릿은 `Model`이라는 루트 `컴포넌트`를 설정하며, 이 버튼을 클릭하면 자동으로 업데이트되는 버튼을 표시합니다.
애플리케이션을 실행시키고 페이지의 `<body>`태그에 적용시키는 `main()`안의 `yew::start_app::<Model>()`을 각별히 유의해야 합니다.

만약 애플리케이션을 동적속성과 함께 실행하고 싶다면, 아래와 같이 작성하면 됩니다.

`yew::start_app_with_props::<Model>(..)`.

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

## 애플리케이션 실행

아직 [Trunk](https://github.com/thedodd/trunk)를 설치하지 않으셨다면, 지금 설치하셔야 합니다.

```bash
cargo install trunk wasm-bindgen-cli
```

아직 `wasm32-unknown-unknown`를 설치하지 않았다면 target에 추가해 주어야 합니다.
Rustup을 이용하여 설치하려면:

```bash
rustup target add wasm32-unknown-unknown
```

이제 남은 것은 다음을 실행하는 것입니다.

```bash
trunk serve
```

해당 명령어는 변경사항이 생길 때마나 앱을 재구동시키는 개발 서버를 실행할 것입니다.



## 문제 해결

* Trunk 설치 실패:
  openssl 개발 패키지가 설치되어 있는지 확인합니다.
  예시로는 Ubuntu의 libssl-dev 또는 Fedora의 openssl-dev가 있습니다.
