---
title: "최적화와 모범사례"
sidebar_label: 최적화
description: "앱을 더 빠르게 만들 수 있는 방법을 소개합니다."
---

## 효율적으로 스마트 포인터 사용하기

**참고: 이 문단에 나타나는 용어들을 잘 모르겠다면 Rust book에
 [스마트 포인터](https://doc.rust-lang.org/book/ch15-00-smart-pointers.html)에 대한 좋은 챕터가 있습니다.** 

다시 렌더링 할때마다 많은 양의 props을 복제하는 것을 피하기 위해 스마트 포인터를 이용하여 데이터 그 자체가 아닌 레퍼런스를 복제할 수 있습니다.
props이나 자식 컴포넌트 속 데이터를 실제 데이터가 아닌 레퍼런스로 전달하면 그 데이터를 자식 컴포넌트에서 수정해야할 필요가 있을 때까지
모든 데이터 복제를 피할 수 있습니다. 자식 컴포넌트에서는 `Rc::make_mut`로 복제하고 수정하고 싶은 데이터에 대한 mutable한 레퍼런스를 얻을 수 있습니다. 

이는 props 변경점으로 인해 다시 렌더링해야 할 지 정해야 할 때 `Component::changed`에게 몇 가지 이점을 안겨줍니다.
왜냐하면 값을 비교하는 대신 내부 포인터 주소(데이터가 저장된 기계의 메모리 위치)를 비교할 수 있습니다.
두 포인터가 같은 데이터를 가리킨다면 각 포인터가 가리키는 데이터의 값도 반드시 같습니다.
그 반대의 경우는 꼭 그렇지 않을 수도 있다는 점에 유의하세요! 두 포인터 주소가 다르더라도 내부 데이터는 같을 수도 있습니다.
그런 경우에는 내부 데이터도 비교해야 합니다.

이러한 비교를 할려면 `PartialEq`가 아닌 `Rc::ptr_eq`를 사용해야합니다(`PartialEq`는 자동적으로 데이터 비교를 `==` 연산자로 합니다).
Rust 문서에 [`Rc::ptr_eq`에 대한 자세한 내용](https://doc.rust-lang.org/stable/std/rc/struct.Rc.html#method.ptr_eq) 이 있습니다.

이러한 방식의 최적화는 `Copy`를 구현하지 않는 데이터에 가장 유용합니다.
데이터를 큰 비용 없이 복사할 수 있다면 스마트 포인터 안에 넣을 가치가 없습니다. `Vec`이나 `HashMap`, `String`처럼 무거운 구조체의 경우 
스마트 포인터를 사용하는 것으로 성능을 향상시킬 가능성이 높습니다.

이러한 최적화는 자식 컴포넌트에서 절대 업데이트 되지 않을 때 가장 효과적이며 부모에서도 간혈적으로 업데이트 된다면 더 좋습니다.
이로 인해 `Rc<_>`로 순수 컴포넌트에서 속성 값을 감싸는 것도 좋은 방법입니다.

하지만 자식 컴포넌트에서 데이터에서 직접 복제해야하지 않는 이상 이러한 최적화는 쓸모 없을 뿐더러 불필요하게 레퍼런스 카운팅에 대한 비용도 추가합니다.
Yew props은 이미 레퍼런스 카운팅되며 내부적으로 데이터 복제는 하지 않습니다.

## 뷰 함수

코드 가독성 문제로 인해 `html!`의 일부를 따로 함수로 옮기는 것도 대부분의 경우엔 적절합니다.
이는 들여쓰기의 양을 줄이기 때문에 코드를 더 읽기 쉽게할 뿐만 아니라 좋은 디자인 패턴을 사용하도록 유도합니다.
특히 그 함수들이 다른 곳에서도 여러번 호출할 수 있게 되어 작성해야하는 코드 량을 줄여 컴포저블한 애플레이션을 만들도록 유도합니다.

## 순수 컴포넌트

순수 컴포넌트란 상태를 수정하지 않는 컴포넌트입니다. 콘텐츠를 보여주고 다른 일반적인 mutable한 컴포넌트로 메시지를 전파하는데만 쓰입니다.
뷰 함수와 다른 점은 `html!` 매크로 안에서 표현식 문법\(`{some_view_function()}`\)이 아닌
컴포넌트 문법\(`<SomePureComponent />`\)으로 사용되고 구현에 따라 memoization하여 동일한 props으로 중복적으로 렌더링하지 않습니다.
(이는 함수가 한번 호출되면 값이 저장되어 같은 인수로 호출되면 값을 계산할 필요 없이 첫 번째 함수 호출에서 저장된 값을 리턴한다는 뜻입니다.)
이때 Yew는 props을 내부적으로 비교하여 포롭이 변경될 때만 UI를 다시 렌더링합니다.

## 워크스페이스로 컴파일 시간 줄이기

Yew를 사용하는 것에 대한 가장 큰 약점은 주로 Yew 앱을 컴파일 하는데 시간이 오래 걸린다는 것입니다.
프로젝트를 컴파일하는데 걸리는 시간은 `html!` 매크로로 넘겨진 코드 량에 관련 있는걸로 보입니다.
이는 작은 프로젝트에서는 일반적으로 큰 문제가 되지 않습니다만
큰 애플리케이션에서는 코드를 여러 crate로 나눠서 각 애플리케이션 코드 변경점으로 인한 컴파일러의 작업량을 줄일 만 합니다.

한 가지 유효한 접근법은 메인 crate에서 라우팅/페이지 선택을 처리하고 각 페이지를 각 crate로 분리하는 겁니다.
여기서 각 페이지는 개별적인 컴포넌트일수도, 혹은 `Html`를 생성하는 큰 함수일수도 있습니다.
각 crate간에 공유되는 코드는 따로 프로젝트 전반적으로 의존되는 crate로 분리할 수 있습니다.
최상의 경우 매 컴파일마다 모든 코드를 빌드하는 대신 페이지 crate 하나랑 메인 crate 하나만 빌드하면 됩니다.
"공용" crate의 일부를 수정하는 최악의 경우엔 처음과 같습니다: 공용 코드에 의존하는 모든 코드를 컴파일하는 것입니다. 

메인 crate가 너무 무겁거나 깊게 중첩된 페이지를 개발해야한다면\(예를 들어 다른 페이지 안에서 렌더링되는 페이지\),
예시 crate를 통해 간소화된 메인 페이지를 만들고 그 위에 작업중인 컴포넌트를 렌더링 할 수 있습니다.

## 바이너리 크기 줄이기

* Rust 코드 최적화
  * `wee_alloc` \( 소형 할당자 \)
  * `cargo.toml` \( 릴리스 프로파일 정의 \)
* `wasm-opt`로 wasm code 코드 최적화

**참고: 바이너리 크기 줄이기에 대한 더 많은 정보는
[Rust Wasm 책](https://rustwasm.github.io/book/reference/code-size.html#optimizing-builds-for-code-size) 에서 찾을 수 있습니다.**

### wee\_alloc

[wee\_alloc](https://github.com/rustwasm/wee_alloc) 은 Rust 바이너리에 쓰이는 할당자보다 훨씬 작은 할장자 입니다.
이것으로 기본 할당자를 대체하면 성능, 메모리 오버헤드의 댓가로 Wasm 파일 크기가 줄어듭니다.

더 느린 성능, 메모리 오버헤드는 기본 할당자를 사용하지 않음으로써 줄어드는 크기에 비하면 작은 편입니다.
더 작은 파일 크기는 곧 페이지 로드가 빨라진다는 것이므로 애플리케이션 작업이 많지 않은 이상 일반적인 경우 기본 할당자대신 이 할당자를 쓰는것이 추천됩니다.

```rust ,ignore
// `wee_alloc`를 전역 할당자로 사용하기.
#[global_allocator]
static ALLOC: wee_alloc::WeeAlloc = wee_alloc::WeeAlloc::INIT;
```

### Cargo.toml

`Cargo.toml`의 `[profile.release]`에서 사용가능한 설정들로 릴리스 빌드가 더 작아지도록 설정할 수 있습니다.


```text
[profile.release]
# 바이너리에 들어가는 코드 줄이기
panic = 'abort' 
# 모든 코드 베이스 최적화 ( 더 많은 최적화, 더 느린 빌드 시간 )
codegen-units = 1
# 크기 최적화 ( 더 공격적인 최적화 )
opt-level = 'z' 
# 크기 최적화 
# opt-level = 's' 
# 전체 프로그램 분석을 통한 링크 시간 최적화
lto = true
```

### wasm-opt

더 나아가 `wasm` 코드의 크기를 줄일 수 있습니다.

Rust Wasm 책에 바이너리 크기를 줄이는 것에 대한 문헌이 있습니다:
[Shrinking .wasm size](https://rustwasm.github.io/book/game-of-life/code-size.html)

* `wasm-pack`로 릴리스 빌드에서 `wasm` 코드에 대한 기본 최적화하기.
* `wasm` 파일에 직접 `wasm-opt`를 실행하기.

```text
wasm-opt wasm_bg.wasm -Os -o wasm_bg_opt.wasm
```

#### yew/examples/ 'minimal' 예시의 빌드 크기  

주의: `wasm-pack`은 Rust 코드 최적화와 Wasm 코드 최적화를 모두 합니다. 여기서는 `wasm-bindgen`를 사용하여 Rust 크기 최적화를 하지 않았습니다.

| 사용된 툴 | 크기 |
| :--- | :--- |
| wasm-bindgen | 158KB |
| wasm-bindgen + wasm-opt -Os | 116KB |
| wasm-pack | 99 KB |

## 추가 자료:
 * [Rust book 속 스마트 포인터에 대한 챕터](https://doc.rust-lang.org/book/ch15-00-smart-pointers.html)
 * [Rust Wasm 책 속 바이너리 크기를 줄이는 것에 대한 정보](https://rustwasm.github.io/book/reference/code-size.html#optimizing-builds-for-code-size)
 * [Rust 프로파일에 대한 문서](https://doc.rust-lang.org/cargo/reference/profiles.html)
 * [binaryen 프로젝트](https://github.com/WebAssembly/binaryen)
