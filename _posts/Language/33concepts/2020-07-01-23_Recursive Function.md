---
layout: post
title: "JS 33 concepts - 23_Recursive Function"
author: "Oizys18"
categories: [Language]
comments: true
tags: [Javascript,JS33Concepts,Language,]
---


- 함수 그 자체에서 자기 자신을 호출하는 함수 
- 자바스크립트에서 재귀 함수를 구성 할때는 `Leave Event`가 있어야 합니다.
  - `Leave Event`?
    함수가 재귀 루프를 종료 할 수 있게하는 제어문
    이것은 if문, 삼항인수, switch문 등이 될 수 있다.
- 프랙털 수학, 정렬 또는 복잡한 또는 비선형 데이터 구조의 노드 탐색과 같은 반복 분기와 관련된 문제를 해결하는 데
  가장 효과적입니다.
- 함수형 프로그래밍 언어에서 재귀를 선호하는 한 가지 이유는 로컬 변수를 사용하여 상태를 설정 및 유지 관리 할 필요가 없는 코드를 생성 할 수 있기 때문입니다.
- 재귀 함수는 주어진 입력에 대해 구체적이고 일관된 반환 값을 가지고 외부 변수 상태에 대한 부작용이 없는 순수한 방식으로 작성하기 쉽고 자연스럽게 테스트하기 쉽습니다.

### 작동방식

- 함수의 최종 결과가 반환되면 단계적으로 중첩된 함수호출 그룹이 모두 반환된다. **재귀는 단순히 중첩 된 함수 호출 그룹이다. 중첩 함수를 사용하면 `most inner` 중첩 함수가 먼저 반환됩니다.**

## 3가지 특징

### 1. A Termination Condition 종료조건

`Leave event` : `if (뭔가 나쁜 일이 생겼다) {STOP};` 종료 조건은 보험이다... 비상 브레이크처럼 생각하십시오. 입력이 잘못되면 재귀가 실행되지 않도록 합니다. 팩토리얼 예제에서 `if (x <0) return;` 은 종료 조건입니다. 음수를 계승 할 수 없으므로 음수가 입력 된 경우 재귀를 실행하고 싶지 않습니다.

### 2. A Base Case 기본조건

 `if (이러한 일) {예! 끝났다.};` 기본 사례는 재귀를 중지한다는 점에서 종료 조건과 유사합니다. 그러나 종료 조건은 나쁜 데이터에 대한 포괄적 인 것임을 기억하십시오. 기본 사례는 재귀 함수의 목표입니다. 기본 사례는 일반적으로 `if` 문 안에 있습니다. 계승 예에서 `if (x === 0) return 1;` 이 기본 사례입니다. x 를 0으로 낮추면 계승을 결정하는 데 성공했습니다!

### 3. The Recursion 

 우리의 함수 자체를 호출합니다. 계승 예에서 `return x * factorial (x — 1);` 은 실제로 재귀가 발생하는 곳입니다. 우리는 숫자 `x` 의 값에 `factorial (x-1)` 이 평가하는 모든 값을 곱한 값을 반환합니다.

## 재귀 작성법

### Leave event 작성법

```js
// for-loop로 작성
function countDown(n) {
  for(let i=n; i >=0;i--) {
   console.log(i);
  } 
}
countDown(5);

// 재귀로 작성
function countDown(n) {
  console.log(n);
  if(n >= 1) countDown(n-1);
}
countDown(5);
```

### DOM 조작과 Traverse 순회

```js
// 위에서 아래로
function Traverse(ele, callback) {
  callback(ele);
  ele = ele.firstChild;
  while (ele) {
    Traverse(ele, callback);
    ele = ele.nextSibling;
  }
}

// 아래에서 위로 
function TraverseUp(ele, callback) {
  callback(ele);
  ele = ele.parentNode;
  while (ele) {
    TraverseUp(ele, callback);
    ele = ele.prevSibling;
  }
}
TraverseUp(document.getElementById('outer'), (ele) => console.log(ele));
```

### [Tail Call Optimization (꼬리 호출 최적화)](https://velog.io/@yesdoing/%EA%BC%AC%EB%A6%AC-%EB%AC%BC%EA%B8%B0-%EC%B5%9C%EC%A0%81%ED%99%94Tail-Call-Optimization%EB%9E%80-2yjnslo7sr)

- 재귀와 같이 연달아서 서브루틴 호출이 발생하는 상황에서 `호출된 함수`가 `호출을 한 함수`의 `반환지점`을 가지고 있어야하는데 이 때 `호출을 한 함수`가  메모리(인자,변수 등)을 가지고 있지 않으면 `호출을 한 함수`로 되돌아올 필요가 없고 아예 최초 함수 호출지점으로 값을 반환하면 된다. 
  - `== 서브루틴 체인이 발생할 때 실행 컨텍스트를 넣지 않는 것 `
- 이렇게 하면 JS의 콜스택에 메모리를 적게 쌓아서 최적화가 가능하다.

#### 문제점

- 현대적인 JavaScript 구현의 한 가지 문제점은 재귀 함수가 무기한으로 쌓이지 않고 엔진 용량을 초과 할 때까지
  메모리에서 쌓이는걸 방지할 표준 방법이 없다는 것입니다.
  JavaScript 재귀 함수는 매번 호출 된 위치를 추적해야 올바른 시점에서 다시 시작할 수 있습니다.
- Haskell 및 Scheme과 같은 많은 기능 언어에서 이것은 꼬리 호출 최적화라는 기술을 사용하여 관리됩니다.
  꼬리 호출 최적화를 사용하면 재귀 함수의 각 연속 사이클이 메모리에 쌓이지 않고 즉시 발생합니다.

#### 구현

- 꼬리호출 : 서브루틴의 호출을 프로시저(procedure)의 마지막 행위로 수행하는 것

- 최적화 : Return을 할 때 함수를 호출하면 "호출이 된" 함수에서 "호출을 한" 함수로 돌아오는 반환 지점을 가지고 있어야 합니다. 만약 "호출을 한" 함수가 메모리(실행 컨텍스트 - Argument, Local Variable)를 가지지 않는다면 "호출을 한" 함수로 돌아올 필요가 없으며, 함수들이 한 번씩 "호출이 되고" 마지막으로 "호출이 된" 함수는 최초 함수 호출 지점으로 값을 반환하는 것을 뜻합니다.

  이것이 가능하기 위해서는, return에서는 연산자를 사용하면 안 됩니다.(언어 스펙에서 지정한 스택에 메모리를 쌓지 않는 연산자는 사용 가능합니다.)

  서브루틴의 내부 연산에 필요한 상태나 값들은 전부 서브루틴의 매개인자로 넘겨야 합니다.

- Good

```js
// 삼항 연산자는 JS 스펙에 정의된 콜스택에 메모리가 잡히지 않는 연산자입니다.
const factorial = (x, acc=1) => {
	return ( x <= 1 ? acc : factorial(x-1, x*acc));
};
```

![img](https://media.vlpt.us/post-images/yesdoing/b7d00c40-da89-11e8-8483-39fd55313fb0/image.png)

- Bad

```js
// 삼항 연산자는 JS 스펙에 정의된 콜스택에 메모리가 잡히지 않는 연산자입니다.
const factorial = (x) => {
	return ( x <= 0 ? 1 : x * factorial(x-1));
};
```

![img](https://media.vlpt.us/post-images/yesdoing/bd3ff460-da89-11e8-8483-39fd55313fb0/image.png)

### [Trampoline Functions (트램폴린 함수들)](https://en.wikipedia.org/wiki/Trampoline_(computing))

- JS가 안전한 방식으로 재귀함수를 수행하도록 하는 방법
- 일반적으로 안전을 위해 성능이 저하되고, 코드 복잡도가 올라간다. 
- 재귀 호출이 함수 내에서 자신을 또 호출하고, 호출하고 호출하는 식으로 내려가다가 다시 리턴하고 리턴하는 식으로 올라오는 것을 반복한다면, 트램폴린은 어떤 함수를 호출하고 그 결과를 받아서 다시 그 함수를 호출하는 식으로 일종의 루프를 도는 식으로 똑같은 함수에서 트램폴린을 타듯이 뜀뛰는 것을 의미한다.

### 익명재귀

```js
(
  (
    (f) => f(f) // 이 함수는 아래 함수를 인자 F로 받아 그 자체로 부릅니다.
  )
  (
    (f) => // 위의 함수에 대한 인수로 이 전체 함수가 전달됩니다.
      (l) => { //`2. 3에서 불려지고 있다.
          	// 가장 외부 스코프를 만족시켜 입력배열을 l 인자로 전달하는 4 함수를 반환하는 결과를 낳습니다.
        console.log(l)
        if (l.length) f(f)(l.slice(1))
        console.log(l)
      }
  )
)
(
  [1, 2, 3] // 입력 배열 '[1, 2, 3]이 가장 바깥쪽 스코프로 전달됩니다.
)

```



## 참고

- [꼬리재귀 최적화와 트램폴린](https://soooprmx.com/archives/6537)
- [신기한 트램폴린](https://www.it-swarm.dev/ko/javascript/%EC%99%9C-%ED%8A%B8%EB%9E%A8%ED%8E%84%EB%A6%B0%EC%9D%B4-%EC%9E%91%EB%8F%99%ED%95%A9%EB%8B%88%EA%B9%8C/l966966505/)