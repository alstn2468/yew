---
title: "trunk 사용하기"
---

## 설치하기

```bash
cargo install trunk wasm-bindgen-cli
```

## 사용법

Trunk를 이용하여 Yew 애플리케이션을 구축하는 간단한 가이드 ["Build a sample app"](../build-a-sample-app.md)를 확인해 보세요.

또한 Trunk로 만들어진 [예시](https://github.com/yewstack/yew/tree/master/examples)들이 구동되는 것을 직접 확인해 보세요.

Trunk는 일종의 구성 파일 역할을 하는 `index.html`파일을 기반으로 애플리케이션을 구축합니다.
이 도구는 `wasm-pack`과는 다르게 애플리케이션을 구축할 수 있도록 설계되었습니다.
이 뜻은 라이브러리 타겟으로 `cdylib`를 추가할 필요 없이 `main`함수를 엔트리 포인트로 사용할 수 있다는 것입니다.

간단한 Yew 애플리케이션을 개발하기 위해서는 프로젝트의 루트 폴더에`index.html`파일이 있기만 하면 됩니다 :

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Yew App</title>
  </head>
</html>
```

Trunk CLI는 여러 유용한 명령어들을 제공하지만 개발 단계에서는 `trunk serve`가 당연 가장 유용한 명령어입니다.
변경 사항을 감지하면 애플리케이션을 자동으로 재구축하는 로컬 서버를 실행할 것 입니다.

애플리케이션을 배포할 준비가 되었다면, `trunk build --release`를 실행하기만 하면 됩니다.

이 요약에서는 Trunk의 모든 기능을 다루고 있지 않습니다.
[README](https://github.com/thedodd/trunk)를 꼭 확인하세요!
