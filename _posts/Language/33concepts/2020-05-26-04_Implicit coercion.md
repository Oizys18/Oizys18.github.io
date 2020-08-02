---
layout: post
title: "JS 33 concepts - 04_Implicit coercion"
author: "Oizys18"
categories: [Language]
comments: true
tags: [Javascript,JS33Concepts,Language,]
---
## 목차
* TOC
{:toc}
* * *

- 예시

    ```jsx
    3 * "3" //9
    1 + "2" + 1 //121

    true + true //2
    10 - true //9

    const foo = {
      valueOf: () => 2
    }
    3 + foo // 5
    4 * foo // 8

    const bar = {
      toString: () => " promise is a boy :)"
    }
    1 + bar // "1 promise is a boy :)"

    4 * [] // 0
    4 * [2] // 8
    4 + [2] // "42"
    4 + [1, 2] // "41,2"
    4 * [1, 2] // NaN

    "string" ? 4 : 1 // 4
    undefined ? 4 : 1 // 1
    ```

## Coercion

### 명시적

- 개발자에 의해 의도적으로 값의 타입을 변환하는 것을 `명시적 타입 변환(Explicit coercion)` 또는 `타입 캐스팅(Type casting)`이라 한다.

### 암묵적

- 동적 타입 언어인 자바스크립트는 개발자의 의도와는 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기도 한다. 이를 `암묵적 타입 변환(Implicit coercion)` 또는 `타입 강제 변환(Type coercion)`이라고 한다.

### 실 사용시?

- 암묵적 형변환을 지양하는 것은 옳지 않다. 명시적 타입 변환보다 암묵적 타입 변환이 가독성 면에서 더 좋을 수도 있다. **중요한 것은 코드를 예측할 수 있어야 한다는 것이다. 동료가 작성한 코드를 정확히 이해할 수 있어야 하고 자신의 코드는 타인에 의해 쉽게 이해될 수 있어야 한다.**

### Why?

- JS가 Forgiving Language라서 → Web의 동작을 다루니까 최대한 에러가 발생해서 작동을 멈추는 일을 줄여야 함
- JS는 Type을 지정하지 않는 언어 → C, C++ 등의 몇몇 low level 언어와 달리 JS는 각 데이터 유형을 사용할 때 미리 데이터 타입을 지정하지 않는다.

## 암묵적 형변환 (Implicit coercion)

- 암묵적 타입 변환은 변수 값을 재할당해서 변경하는 것이 아니라 자바스크립트 엔진이 표현식을 에러없이 평가하기 위해 *기존 값을 바탕으로 새로운 타입의 값을 만들어 단 한번 사용하고 버린다.*
- 자바스크립트 엔진은 표현식을 평가할 때 문맥, 즉 컨텍스트(Context)에 고려하여 암묵적 타입 변환을 실행한다.
- 암묵적 타입 변환이 발생하면 **문자열, 숫자, 불리언과 같은 원시 타입 중 하나로 타입을 자동 변환한다.**
- 원시타입과 object에 대한 변환 로직은 다르게 작용하지만, 원시타입과 object 모두 형변환될 수 있다.
- `==`는 `+`처럼 암묵적 형변환을 유발한다. `===`를 사용하면 형변환이 방지된다.

## 원시타입의 암묵적형변환

### to String

- `+` 연산자는 `수학적 덧셈`말고도 `문자열을 연결하는 기능(Concatenation)`도 수행한다.

### to Number

- 비교연산자들 (`>`, `<`, `<=`,`>=`)
- 비트연산자들 ( `|` `&` `^` `~`)
- 산술 연산자들(`-` `+` `*` `/` `%` ). 참고, 이진 연산자 +는 문자열을 만나면 숫자형변환이 일어나지 않는다.
- unary `+` operator
- 느슨한 동등연산자 `==` (incl. `!=`).
- 특수규칙
    1. `null` 또는 `undefined`에 `==`을 적용하면 숫자 변환은 일어나지 않는다. `null`은 `null` 또는 `undefined`에 해당하며 다른 어떤 것과 같지 않다.
    2. `NaN`은 그 자체와도 비교가 되지 않는다.

### to Boolean

- 논리적 맥락 혹은 논리연산자(`||`, `&&`, `!`)에 의해 발생한다.
- 변환의 결과는 `true` 혹은 `false` 단 두 가지다.
- 빈 primitive type은 `false`, 빈 object는 `true`다.
- 사실 모두 `Number`로 변환된 후 비교된다.
    - 즉 `==` 비교 시 `false`는 `0`, `true`는 `1`로 변환된 후 비교된다.

## Object 형변환

- `object → 원시값 → 최종타입으로 변환`의 단계를 거친다.
- object는 모두 `[[ToPrimitive]]`를 상속받기 때문에 `toString` `toNumber` 로 형변환할 수 있다.