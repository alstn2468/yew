---
title: "Refs"
description: "대역 외 DOM 접근"
---

`ref` 키워드는 HTML 요소 혹은 DOM `Element`를 가지는 컴포넌트 내부에서 사용되며 `view` lifecycle 대역 밖에서도 DOM을 변화시킬 수 있습니다.

해당 키워드는 캔버스 요소들을 보존하거나 다른 섹션의 페이지를 스크롤 할 때 유용합니다.

예를 들어, `view`로 부터 렌더링 된 이후 컴포넌트의 `rendered` 메소드에서 `NodeRef`를 사용하여 캔버스 요소를 호출할 수 있습니다.

문법:

```rust
use yew::{html, NodeRef, web_sys::Element};

// In create
self.node_ref = NodeRef::default();

// In view
html! {
    <div ref=self.node_ref.clone()></div>
}

// In rendered
let has_attributes = self.node_ref.cast::<Element>().unwrap().has_attributes();
```

## 관련 예시

- [Node Refs](https://github.com/yewstack/yew/tree/v0.18/examples/node_refs)
