---
title: "자식"
---

## 기본적인 사용법

_대부분,_ 컴포넌트가 자식을 가져야 할 때, 자식이 무슨 타입을 가지는지 상관하지 않을 것입니다.
이러한 경우, 아래의 예시로 충분합니다.

```rust
use yew::{html, Children, Component, Html, Properties};

#[derive(Properties, Clone)]
pub struct ListProps {
    #[prop_or_default]
    pub children: Children,
}

pub struct List {
    props: ListProps,
}

impl Component for List {
    type Properties = ListProps;
    // ...

    fn view(&self) -> Html {
        html! {
            <div class="list">
                { for self.props.children.iter() }
            </div>
        }
    }
}
```

## 고급적인 사용법

### 타입을 가진 자식

컴포넌트의 타입을 자식으로 전달해야 하는 경우,
`yew::html::ChildrenWithProps<T>`를 사용하실 수 있습니다.

```rust
use yew::{html, ChildrenWithProps, Component, Html, Properties};

// ...

#[derive(Properties, Clone)]
pub struct ListProps {
    #[prop_or_default]
    pub children: ChildrenWithProps<Item>,
}

pub struct List {
    props: ListProps,
}

impl Component for List {
    type Properties = ListProps;
    // ...

    fn view(&self) -> Html {
        html! {
            <div class="list">
                { for self.props.children.iter() }
            </div>
        }
    }
}
```

### Enum 타입을 가진 자식

물론, 제한된 상황에 자식을 다른 컴포넌트에 불러와야 할 때도 있습니다. 이 경우, Yew를 상황에 맞게 사용할 필요가 있습니다.

[`derive_more`](https://github.com/JelteF/derive_more) crate는 더 나은 환경을 위하여 사용됩니다.
만약 사용하길 원치 않을 경우, 직접 `From`을 각 상황에 맞게 사용하시면 됩니다.

```rust
use yew::{
    html, html::ChildrenRenderer, virtual_dom::VChild,
    Component, Html, Properties
};

// `derive_more::From`는 `Item`을 위해
// `From<VChild<Primary>>`와 `From<VChild<Secondary>>`를 자동으로 구현합니다!

#[derive(Clone, derive_more::From)]
pub enum Item {
    Primary(VChild<Primary>),
    Secondary(VChild<Secondary>),
}

// 이제, `Into<Html>`를 사용하여 Yew가 `Item`을 렌더링 하도록 했습니다.
impl Into<Html> for Item {
    fn into(self) -> Html {
        match self {
            Self::Primary(child) => child.into(),
            Self::Secondary(child) => child.into(),
        }
    }
}

#[derive(Properties, Clone)]
pub struct ListProps {
    #[prop_or_default]
    pub children: ChildrenRenderer<Item>,
}

pub struct List {
    props: ListProps,
}

impl Component for List {
    type Properties = ListProps;
    // ...

    fn view(&self) -> Html {
        html! {
            <div class="list">
                { for self.props.children.iter() }
            </div>
        }
    }
}
```

### 선택적 타입이 포함된 자식

자식 컴포넌트의 특정 타입이 단독 선택적 타입을 가질 수 있도록 할 수 있습니다.:

```rust
use yew::{html, virtual_dom::VChild, Component, Html, Properties};

#[derive(Clone, Properties)]
pub struct PageProps {
    #[prop_or_default]
    pub sidebar: Option<VChild<PageSideBar>>,
}

struct Page {
    props: PageProps,
}

impl Component for Page {
    type Properties = PageProps;
    // ...

    fn view(&self) -> Html {
        html! {
            <div class="page">
                { self.props.sidebar.clone().map(Html::from).unwrap_or_default() }
                // ... 페이지 내용
            </div>
        }
    }
}

// 페이지 컴포넌트는 사이드 바의 호출 여부에 상관없이 불러올 수 있습니다.:


// 사이드 바가 제외된 페이지
html! {
    <Page />
}


// 사이드 바가 포함된 페이지
html! {
<Page sidebar=html_nested! {
    <PageSideBar />
    } />
}
```
