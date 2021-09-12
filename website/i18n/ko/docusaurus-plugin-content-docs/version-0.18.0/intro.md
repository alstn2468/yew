---
title: "소개"
slug: /
---

## Yew란 무엇인가요?

**Yew**는 [WebAssembly](https://webassembly.org/)를 통해 멀티쓰레드 기반의 프론트엔드 웹 애플리케이션을 개발하기 위한 현대적인 [Rust](https://www.rust-lang.org/) 프레임워크입니다.

- **컴포넌트 기반**의 프레임워크이므로 사용자와 상호작용하는 UI를 쉽게 만들 수 있습니다. [React](https://reactjs.org/)나
  [Elm](https://elm-lang.org/)과 같은 프레임워크가 익숙한 개발자들은 Yew가 낯설지 않을 것입니다.
- DOM API 요청을 최소화하고 web worker를 통해 백그라운드 쓰레드로 처리를 넘김으로써 **놀라운 성능**을 이뤄낼 수 있습니다.
- **JavaScript 상호운용성**을 제공합니다. 기존 JavaScript 애플리케이션과의 통합과 NPM 패키지의 활용을 가능하게 합니다.

### 함께 합시다 😊

- [깃헙의 이슈 페이지](https://github.com/yewstack/yew/issues)에서 기능에 대해 토론과 버그 신고를 할 수 있습니다.
- PR도 환영합니다. 도와주고 싶으시다면 [good first issues](https://github.com/yewstack/yew/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22) 라벨이 붙은 이슈들을 확인해주세요!
- 저희의 [디스코드 채팅방](https://discord.gg/VQck8X4)도 굉장히 활성화되어있으며 질문하기 좋은 장소입니다.

### 준비되셨나요?

아래 링크를 클릭하여 여러분의 첫 Yew 애플리케이션을 만들어보고, 예시 프로젝트로 공부해보세요.

[Getting started](getting-started/project-setup.md)

### 아직도 망설이십니까?

Yew는 최첨단 기술을 기반으로 하며, 미래에 표준이 될 프로젝트를 만들고 싶은 개발자들에게 딱입니다. 저희는 Yew가 기반으로 하는 기술들의 속도와 안정성은 미래의 빠르고 유연한 웹 애플리케이션들의 표준이 될 것이라 믿습니다.

#### 잠깐, 왜 WebAssembly죠?

WebAssembly\(_Wasm_\)는 Rust에서 컴파일될 수 있는 이식성 높은 저수준 언어입니다. 브라우저들에서 성능저하 없이 동작하며, JavaScript와 함께 운용될 수 있으며, 현대의 모든 주요 브라우저들에서 지원됩니다. 애플리케이션에서 WebAssembly를 제대로 활용할 수 있는 방법에 대해 알고 싶다면 이 [사례 목록](https://webassembly.org/docs/use-cases/)을 참고해보세요.

다만 WebAssembly를 사용하는 것은 \(아직\) 웹 애플리케이션 성능 개선에 있어 만병통치약인 것만은 아닙니다. 현재로서 WebAssembly의 DOM API를 사용하는 것은 JavaScript에서 직접 호출하는 것보다 느립니다.
이 점은 [WebAssembly 인터페이스 타입 기획안](https://github.com/WebAssembly/interface-types/blob/master/proposals/interface-types/Explainer.md)에서 해결하는 것을 목표로 하고 있는 일시적인 이슈입니다. 이 기획안에 대해 더 알고 싶다면 Mozilla에서 나온 이 [기사](https://hacks.mozilla.org/2019/08/webassembly-interface-types/)에서 확인해볼 수 있습니다.

#### 아하! 근데 왜 Rust인가요?

Rust는 미칠듯이 빠르며 풍부한 타입 체계와 소유권 모델을 사용하므로 안정적입니다. 물론 입문하기 어려울 수 있지만 충분히 배워볼 가치는 있습니다. Rust는 스택오버플로우의 개발자 설문에서 5년 연속으로 가장 사랑받는 프로그래밍 언어로 선정되었습니다:
[2016](https://insights.stackoverflow.com/survey/2016#technology-most-loved-dreaded-and-wanted),
[2017](https://insights.stackoverflow.com/survey/2017#most-loved-dreaded-and-wanted),
[2018](https://insights.stackoverflow.com/survey/2018#technology-_-most-loved-dreaded-and-wanted-languages),
[2019](https://insights.stackoverflow.com/survey/2019#technology-_-most-loved-dreaded-and-wanted-languages), [2020](https://insights.stackoverflow.com/survey/2020#most-loved-dreaded-and-wanted).

또한 Rust는 풍부한 타입 체계와 소유권 모델을 통해 개발자들이 더 안전한 코드를 작성할 수 있도록 도와줍니다. JavaScript 코드에서는 찾기 힘들던 경쟁상태의 버그들과는 작별하세요! Rust에서는 앱이 구동하기도 전에 대부분의 버그들이 컴파일러에 의해 잡힐 것입니다. 그리고 걱정마세요. 만약 에러가 발생하더라도 브라우저 콘솔에 에러가 발생한 코드의 위치에 대해 세부적인 기록이 남을 것입니다.

#### 다른 대안은 없나요?

저희는 다른 프로젝트들과 아이디어를 나누고 싶고, Rust라는 신기술의 잠재력을 발휘하기 위해 협력할 수 있다고 믿습니다. 만약 Yew가 별로라면 다음 프로젝트들은 어떨까요?

- [Percy](https://github.com/chinedufn/percy) - _"Rust와 WebAssembly를 활용하여 클라이언트와 서버에서 모두 동작하는 웹 애플리키에션을 개발하기 위한 모듈식 툴킷"_
- [Seed](https://github.com/seed-rs/seed) - _"웹 애플리케이션 개발을 위한 러스트 프레임워크"_
