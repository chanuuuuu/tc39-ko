# Hashbang 문법

이 제안서는 [샤뱅 / 해시뱅](https://en.wikipedia.org/wiki/Shebang_(Unix)) 을 허용하는 일부 CLI JS 호스트들의 현존하고 있는 사용방식을 일치시키기 위함입니다. 몇몇 호스트는 현재 JS 엔진에 전달하기 전에 유효한 JS 소스 텍스트들을 생산하기 위해 해시뱅을 제거합니다. 이는 제거하는 것을 엔진으로 옮길 수 있으며, 작업 방식을 통일하고 표준화할 수 있습니다. 

## 예시

```mjs
#!/usr/bin/env node
// in the Script Goal
'use strict';
console.log(1);
```

```mjs
#!/usr/bin/env node
// in the Module Goal
export {};
console.log(1);
```

## 상태

* 4단계
* [테스트들](https://github.com/tc39/test262/pull/2065)
* [명세서 PR](https://github.com/tc39/ecma262/pull/2816)
* 구현사항 탑재:
    * [Chrome 74](https://www.chromestatus.com/features#milestone%3D74)
    * [Firefox 67](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/Releases/67)
    * [ChakraCore](https://github.com/microsoft/ChakraCore/pull/6145)
    * [Safari 13.1](https://trac.webkit.org/changeset/248826/webkit)
    * Node.js 12.0.0
    * xs
    
## 왜 셔뱅 대신 해시뱅인가요?

해시는 해시태그와 같이 `#`이 포함된 현대 용어와 공통적으로 연관되어있습니다. 검색 가능성을 위해 추가로, "hash sign"이라는 용어는 `#`에 사용되지만, "she sign"는 그렇지 않습니다.

## 왜 오직 소스 텍스트의 시작에만 있나요?

해시뱅 문장들은 다양한 CLI 환경에서 오직 파일의 시작에서만 인터프리터들에게 유효합니다. 다른 위치에서 허용함을 통한 확장된 사용으로는 이득이 없으며, 실제로 CLI 환경에서 픽업되지 않을 수 있기 때문에 혼란이 발생할 수 있습니다. 또한 해시뱅을 다른 곳에서 허용함으로써 `#!`의 문법적 공간이 특정한 종류의 이진 표현일 경우에 겹칠 수 있습니다. 다른 곳에서 줄 주석을 원할 때는 `//`을 사용하세요.

## `eval`이나 `Function` 내부에서의 해시뱅 문법은 어떻게 동작하나요? 

해시뱅 문장들은 오직 [`Script`](https://tc39.es/ecma262/#prod-Script) 나 [`Module`](https://tc39.es/ecma262/#prod-Module) 의 파싱 대상의 시작에서만 유효합니다. `eval`이 `Script`를 사용하기 때문에 해시뱅 문장들을 지원합니다. `Function`는 [`FunctionBody`](https://tc39.es/ecma262/#prod-FunctionBody) 을 사용하기 때문에 지원하지 않습니다.    


## 바이트 순서 표시와의 상호작용

소스 텍스트의 시작에 있는 [바이트 순서 표시](https://en.wikipedia.org/wiki/Byte_order_mark) (BOM) 문자열은 이후의 `#!`이 해시뱅으로 번역되는 것을 방지합니다. 그러나 웹브라우저는 외부 [스크립트](https://html.spec.whatwg.org/multipage/webappapis.html#fetch-a-classic-script) 나 [모듈](https://html.spec.whatwg.org/multipage/webappapis.html#fetch-a-single-module-script) (각각 [`decode`](https://encoding.spec.whatwg.org/#decode) 또는 [`UTF-8 decode`](https://encoding.spec.whatwg.org/#utf-8-decode) respectively) 페칭의 일부로써 이 문자를 제거합니다. 이는 텍스트가 ECMAScript로 번역되기 전에 발생합니다. 이와 같이 브라우저에서 BOM 문자는 외부 스크립트나 모듈에서 "#!" 앞에 올 수 있지만, 인라인 문자는 올 수 없습니다.

일반적으로, BOM 문자는 호스트에 의해 관리되는 것으로 가정하며, 이 제안서를 통해서 특별히 관리되지 않습니다.

## 현재 존재하는 회의 기록

* [3월 2018](https://tc39.github.io/tc39-notes/2018-03_mar-21.html#10iic-hashbang-grammar-for-stage-2)
* [11월 2017](https://tc39.github.io/tc39-notes/2017-11_nov-28.html#10if-interpreterdirective)