---
layout: post
title: "JS 33 concepts - 20_Pure function, Side effect, Mutate"
author: "Oizys18"
categories: [Language]
comments: true
tags: [Javascript,JS33Concepts,Language,]
---
* TOC
{:toc}
* * *
## Pure function 순수함수

- 동일한 인수가 주어지면 항상 동일한 결과를 반환하는 함수
  - 함수실행 중에 변경되는 상태 또는 데이터에 종속되지 않는다. 인자에만 의존해야 합니다.
- 이 함수는 네트워크 요청, 입력 및 출력 장치 또는 데이터 돌연변이와 같은 관측 가능한 부작용을 생성하지 않는다.
- [Clojure](https://clojure.org/about/clojurescript)와 [Scala](https://www.scala-lang.org/)에서 파생된다. 
- Procedure programming
- 전역 변수 또는 클래스의 인스턴스는 **순수 함수** 내에서 합법적 인 변수로 간주되지 않습니다.
  - 변수의 전역 범위는 **순수 함수의 생성에 사용되지 않습니다.**

### 장점 및 필요성

- **Side effect**(외부와의 상호작용)이 없다. (Side effect 자체는 나쁘지 않지만 포함되면 순수함수가 아니다)
  - Making a HTTP request
  - Mutating data
  - Printing to a screen or console
  - DOM Query/Manipulation
  - Math.random()
  - Getting the current time

- **테스트**, **유지보수**와 **리팩토링**의 용이성
- 함수의 결과를 항상 예측할 수 있다.
- 코드 라인을 줄이고 루프를 거의 제거한다.

## Side effect

- side-effect란 현재 실행된 함수의 return 값을 제외한, 함수 바깥의 모든 application state의 변경을 의미한다. 
- side-effect는 나머지 프로그램 로직과 분리해야한다. (분리하여 모듈화 하자!)

- 예시
  - Making a HTTP request
  - Mutating data
  - Printing to a screen or console
  - DOM Query/Manipulation
  - Math.random()
  - Getting the current time
  - Modifying any external variable or object property (e.g., a global variable, or a variable in the parent function scope chain)
  - Writing to a file
  - Writing to the network
  - Triggering any external process
  - Calling any other functions with side-effects



## Mutate 상태변이

- 상태변이

  - 변이는 부작용이다: 프로그램에서 변하는 것이 적을수록 추적해야 할 것이 적어지기 때문에, 프로그램이 더 간단해진다.

  - 불변의 값(*imutable value*)은 한번 만들어 진 다음엔 변경이 되지 않는 값을 말한다. 자바스크립트에선 숫자, 문자열, 불리언과 같은 원시 값이들이 항상 불변 인 값이다. 그러나 배열과 객체 같은 데이터구조는 그렇지 않다.

    변이란 원본 요소를 변경하거나 영향을 주는 것을 의미한다. 우리의 목표는 **원래 요소를 항상 변경되지 않게 하는 것**이다.

  - 변이는 현재 상태만 제공하기 때문에 위험하다. 하나의 참조가 여러번 변경되어도 변경사항이 기록되자 않기 때문이다. 

- [불변객체](https://ko.wikipedia.org/wiki/불변객체)란?
  - 객체 지향 프로그래밍에 있어서 **불변객체**(immutable object)는 생성 후 그 상태를 바꿀 수 없는 객체를 말한다. 반대 개념으로는 가변(mutable) 객체로 생성 후에도 상태를 변경할 수 있다. 
- 불변객체의 장점
  - ~~동시성~~   
  - 인스턴스를 공유하기 위해 저장
  - 사이드 이펙트가 없음
  - 일시적인 커플링이 없음
  - 가독성 향상
  - 메모리 소비 감소
  - 캐시하기 쉬움
  - 테스트하기 쉬움

- JS의 객체는 기본적으로 가변적이다. 즉, 위와 같은 Immutable의 장점을 사용할 수 없다. 단, 가변적인 객체로 Immutable하게 사용하는 방법이 있다.

- cf) [ES6의 `const`는 불변성과 관련없다.](https://medium.com/@khwsc1/es6-const는-불변성-immutability-과-관계-없어요-번역-118d3d50b909): 불변적 바인딩만 할 뿐이다. 

- ES5의 `Object.freeze()` : 객체의 값을 불변으로 만들기 

  - ```js
    const foo = Object.freeze({
        'bar': 27
    });
    foo.bar = 42; // throws a TypeError exception in strict mode;
                  // silently fails in sloppy mode
    console.log(foo.bar);
    // → 27
    ```

  - `Object.freeze()`는 얕다는 점에 유의하세요. 얼어버린(frozen, 중첩된 객체) 객체 내부의 객체 값 또한 변경될 수 있습니다. MDN의 `Object.freeze()`[항목](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)에서는 `deepFreeze()`라는 예시를 통해, 객체의 값을 완벽하게 불변적으로 만드는 구현 방법을 설명하고 있죠.

    여전히, `Object.freeze()` 는 속성-값 쌍에서만 먹힙니다. 지금 현재로써는 `Date`, `Map`, `Set`과 같은 객체들을 완전히 불변적으로 만드는 방법은 없죠.

### 1. 수동적인 방식

- 함수에서 객체를 변경하지 않는다. 

```js
//bad
function save(object){
    object.saved = true;
    return object;
}

//주어진 오브젝트의 속성을 변경하는 대신에 변경된 복사본을 반환하는 함수를 작성합니다.
//better
function save(object){
    let newObject = object.clone();
    newObject.saved = true;
    return newObject;
}
```

- 생성자 이후에 객체를 변경하지 않는다.

```js
// 객체는 참조 입니다. 속성을 변경하지 않으면 명확하지 않은 상태의 상황을 피할 수 있습니다. 
// do this 
let request = {
    method: "GET",
    uri: "http://slemgrim.com"
}

// don't do this
request.method = "POST"
```

- setter를 피한다. (비용이 많이 들어도)
  - setter는 객체를 변경하기 때문에 위의 두 가지 상황에 모두 해당된다. 

### 2. Immutable.js 라이브러리 사용하기

- 페이스북의 [Immutable.js](https://facebook.github.io/immutable-js/)는 상태 불변을 유지하는데 도움을 주는 라이브러리 입니다. 비슷한 방법으로([Mori](https://github.com/swannodette/mori), [seamless-immutable](https://github.com/Lee-hyuna/33-js-concepts-kr/wiki/To-mutate,-or-not-to-mutate,-in-JavaScript) ) 와 같은 각각의 라이브러리를 가지고 있지만 해당 글에서는 Immutable.js을 사용합니다.
- Immutable.js는 `List, Stack, Map, OrderedMap, Set, OrderedSet and Record`와 같은 데이터의 불변을 제공합니다.

```js
import Map from 'immutable';
var request = Map({method: 'GET', uri: 'http://slemgrim.com'});
var post = request.set('method', 'POST');
request.get('method'); // GET
post.get('method'); // POST
```

### 3. ESLint 가변

- [eslint-plugin-immutable](https://github.com/jhusain/eslint-plugin-immutable) 플러그인은 자바스크립트에서 모든 가변을 사용할 수 없습니다. 리드미에 리액트, 리덕스를 언급하지만 이 두개의 플러그인을 제외하고 안전하게 사용할 수 있습니다.

- ### 세가지 규칙을 플러그인에 추가합니다:

  - no-let: let대신에 const를 사용하세요.
  - no-this: ES6 class 사용을 금지합니다.
  - no-mutation: 멤버 표현식의 결과에 값을 할당하는 것을 금지합니다.

### Immutable이 필요한 순간

#### 1.동시성

- 이것은 불변성이 타당한 가장 중요한 이유 입니다. 스레드로부터 안전하기 위해서는 변경되지 앟는 항목을 잠그지 않아도 됩니다. JS에서는 실제 동시성이 없습니다. 대신 Event Loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)가 있습니다. 그래서 이것을 잊어버려도 좋습니다.

#### 2. 사이드 이펙트를 피하세요.

- 객체의 속성을 변경하는 것은 가변적인 것입니다. 가변은 사이드 이펙트에 의해 정의 됩니다. 사이드 이펙트를 피해야 한다고 배웠습니다. 사이드 이펙트가 없다면 함수의 구현을 들여다볼 필요가 없습니다. 코드는 추론하기 쉬우며 예측 가능하고 더 테스트 가능합니다.

#### 3. 함수형 프로그래밍

- 값 또는 레퍼런스를 다루고 있는 지에 대한 생각은 멈춰야 합니다. 불변의 객체는 항상 값입니다 .이것은 함수형 프로그래밍의 핵심 원리중 하나입니다. 함수에 값ㅇ르 전달하면 프로미스를 영원히 유지할 수 있습니다.

### Immutable이 불필요한 순간

#### 1.자주 변경되는 속성

- 실제로 자주 변경되는 객체가 있는 경우 모든 변경에 대해 새 인스턴스를 만드는 것이 가장 좋습니다. 새 인스턴스를 만드는 것이 가장 좋습니다. 이것은 게임과 시뮬레이션에서 특히 두번 반복되는 경우에 해당됩니다. 당신이 게임에서 불변성을 사용할 수 없다는 말은 아니지만, 변경 가능한 객체보다 성능 문제에 빠지기 쉽습니다.

#### 1.거대한 데이터 구조

- 정말 거대한 데이터 트리가 불변 컨테이너로 저장되어 있다면, 메모리 소비량을 확인하는 것이 좋습니다. 물론 가비지 수집은 모든 오래된 객체를 죽일 것이지만 큰 객체를 복사하는 것은 여전히 비용이 듭니다.





## 참고

[JS 함수형 프로그래밍](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0)

[부작용](https://frontendmasters.com/courses/functional-js-lite/side-effects/)

[순수함수](https://velog.io/@jakeseo_me/자바스크립트-개발자라면-알아야-할-33가지-개념-20-자바스크립트-순수함수)