---
layout: post
title: "JS 33 concepts - 34_Iterator, Generator"
author: "Oizys18"
categories: [Language]
comments: true
tags: [Javascript,JS33Concepts,Language]
---
* TOC
{:toc}
* * *


- 원문 33 JS 핵심개념에는 포함되어 있지 않았지만, 학습 중 필요한 내용이라고 판단되어 추가했음.


## 개요
- 반복 개념을 핵심 언어 내로 바로 가져와 [`for...of`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for...of) 루프의 동작(behavior)을 사용자 정의하는 메커니즘
- *Iterator*는 새로운 *Array* 메서드 및 새로운 타입의 컬렉션 (*Set* 및 *Map*)과 결합하여 데이터를 효율적으로 처리할수 있는 핵심 요소

## 반복기 Iterator

- 시퀀스를 정의하고  종료시의 반환값을 잠재적으로 정의하는 객체 

  - *Iterator*는 반복을 위해 설계된 특정 인터페이스가 있는 객체

- 두 개의 속성( `value`, `done`)을 반환하는 next() 메소드 사용하여  객체의 [Iterator protocol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterator_protocol)을 구현

  - `value` == 다음 값, `done` == 반환할 값이 더 이상 없을 때 `true` 
  - *Iterator*는 값 컬렉션 내의 위치에 대한 내부 포인터를 유지하고 `next()` 메서드를 호출할 때마다 다음 적절한 값을 반환합니다.
  - 시퀀스의 마지막 값이 이미 산출되었다면 `done` 값은 true 가 됩니다. 만약 `value`값이 `done` 과 함께 존재한다면, 그것은 반복자의 반환값이 됩니다.
  - 반복자를 생성하면 `next()` 메소드를 반복적으로 호출하여 명시적으로 반복시킬 수 있습니다.
  - 반복자를 반복시키는 것은 일반적으로 한 번씩만 할 수 있기 때문에, 반복자를 소모시키는 것이라고 할 수 있습니다. 마지막 값을 산출하고나서  `next()`를 추가적으로 호출하면 `{done: true}`. 가 반환됩니다.
  - 반복자 내부에 명시적으로 상태를 유지할 필요가 있다.

- next() 설계구조

  ```js
  var iterator = '12'[Symbol.iterator]();
  iterator.next(); // {value: "1", done: false}
  iterator.next(); // {value: "2", done: false}
  iterator.next(); // {value: undefined, done: true}
  ```

  - next 메소드는 arguments 가 없다.
  - next 메소드의 반환자는 done: boolean 과 value: any 를 포함하는 object 를 반환해야 한다.
  - next 메소드의 반복이 끝날때 done 은 true 를 반환해야 한다.

```js
// ES5 예제 items의 길이만큼 i가 증가 
function createIterator(items) {
    var i = 0;
    return {
        next: function() {
            var done = (i >= items.length);
            var value = !done ? items[i++] : undefined;
            return {
                done: done,
                value: value
            };
        }
    };
}
var iterator = createIterator([1, 2, 3]);
console.log(iterator.next());         // "{ value: 1, done: false }"
console.log(iterator.next());         // "{ value: 2, done: false }"
console.log(iterator.next());         // "{ value: 3, done: false }"
console.log(iterator.next());         // "{ value: undefined, done: true }"
// for all further calls
console.log(iterator.next());         // "{ value: undefined, done: true }"

```

```js
// start 부터 end 까지 step만큼 띄어진 정수 시퀀스를 반복한다. 
function makeRangeIterator(start = 0, end = Infinity, step = 1) {
    var nextIndex = start;
    var n = 0;
    
    var rangeIterator = {
       next: function() {
           var result;
           if (nextIndex < end) {
               result = { value: nextIndex, done: false }
           } else if (nextIndex == end) {
               result = { value: n, done: true }
           } else {
               result = { done: true };
           }
           nextIndex += step;
           n++;
           return result;
       }
    };
    return rangeIterator;
}

// 반복자 사용 
var it = makeRangeIterator(1, 4);
var result = it.next();
while (!result.done) {
 console.log(result.value); // 1 2 3
 result = it.next();
}

console.log("Iterated over sequence of size: ", result.value);
```

### Iterables

- `Symbol.iterator` 프로퍼티를 가진 객체
- 객체는 값이 [`for..of`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for...of) 구조 내에서 반복되는 것 같은 그 반복 동작을 정의하는 경우 반복이 가능(**iterable**)합니다.
- [`Array`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array) 또는 [`Map`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map)과 같은 일부 내장 형은 기본 반복 동작이 있지만 다른 형(가령 [`Object`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object))은 없습니다.
- **반복가능**하기 위해서, 객체는 [**@@iterator** 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/@@iterator)를 구현해야 합니다. 즉, 객체( 혹은 그 [프로토타입 체인](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)에 등장하는 객체 중 하나)가 [`Symbol.iterator`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator) 키를 갖는 속성이 있어야 함을 뜻합니다.
- 하나의 iterable은 단 한 번, 혹은 여러번 반복가능합니다. 어떤 순간에 어떻게 사용할 지는 프로그래머에게 달려있습니다. 단 한 번 반복가능한 iterable(e.g. Generator)은 관습적으로 자신의 **@@iterator** 메소드로부터 **this**를 반환합니다. 반면, 여러 번 반복 가능한 iterables은 **@@iterator** 메소드가 호출되는 매 회 새로운 iterator를 반드시 반환해야합니다. 
  - *Generator**가 기본적으로* `Symbol.iterator` *프로퍼티를 할당하므로* *Generator**가 만든 모든* *Iterator**도* *Iterable**입니다.*
- [`String`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String), [`Array`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array), [`TypedArray`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/TypedArray), [`Map`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map) 및 [`Set`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set)은 모두 내장 반복가능 객체입니다, 그들의 프로토타입 객체가 모두 [`Symbol.iterator`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator) 메서드가 있기 때문입니다.

```js
var arr = ['w', 'y', 'k', 'o', 'p'];
var eArr = arr[Symbol.iterator]();
// your browser must support for..of loop
// and let-scoped variables in for loops
// const and var could also be used
for (let letter of eArr) {
  console.log(letter);
}
```

```js
var myIterable = {
    *[Symbol.iterator]() {
        yield 1;
        yield 2;
        yield 3;
    }
}

for (let value of myIterable) { 
    console.log(value); 
}
// 1
// 2
// 3

or

[...myIterable]; // [1, 2, 3]
```

```js
// 일부 문(statement) 및 식(expression)은 iterable합니다, 
// 가령 for-of 루프, spread syntax, yield* 및 해체 할당.
for(let value of ['a', 'b', 'c']){
    console.log(value)
}
// "a"
// "b"
// "c"

[...'abc'] // ["a", "b", "c"]

function* gen(){
  yield* ['a', 'b', 'c']
}

gen().next() // { value:"a", done:false }

[a, b, c] = new Set(['a', 'b', 'c'])
a // "a"

```



## 생성기 Generator 

- *Generator*는 *Iterator*를 반환하는 함수

- 실행이 연속적이지 않은 하나의 함수를 작성함으로서 개발자가 iterative algorithm을 정의할 수 있다.

  - Iterator와 달리 상태유지가 필요없다. 

- `function*` 문법을 사용한다. 

  - 별표가 `function` 바로 앞에 있는지 또는 `*`와 `function` 사이에 공백이 있는지는 중요하지 않습니다. 
  - `generator` 함수가 최초로 호출될 때, 함수 내부의 어떠한 코드도 실행되지 않고, 대신 `generator`라고 불리는 반복자 타입을 반환합니다.
  - `generator`의 **next** 메소드를 호출함으로서 어떤 값이 소비되면, `generator` 함수는 **yield** 키워드를 만날 때까지 실행됩니다. 
  - `generator`함수는 원하는 만큼 호출될 수 있고, 매번 새로운 `generator`를 반환합니다. 하지만, 각  `generator`는 단 한 번만 순회될 수 있을 것입니다.

- `yield` *키워드는* *Generator* *내부에서만 사용할 수 있습니다. 예를 들면 다음과 같이* *Generator* *내부 함수에서 사용하는 것을 포함하여 다른 곳에서* `yield`*를 사용하는 것은 구문 오류입니다.*

  ```js
  function *createIterator(items) {
      items.forEach(function(item) {
          // syntax error
          yield item + 1;   // generator 함수 경계가 아니라 forEach 내부의 함수에 들어있음!
      });
  }
  ```

```js
// generator
function *createIterator() {
    yield 1; // 각 `yield` 문 다음에 실행을 멈춘다.
    yield 2;
    yield 3;
}
// generator는 일반 함수처럼 호출되지만 iterator를 반환합니다.
let iterator = createIterator();
console.log(iterator.next().value);     // 1
console.log(iterator.next().value);     // 2
console.log(iterator.next().value);     // 3

// 위의 Iterator와 동일한 행위를 하는 Generator 
function* makeRangeIterator(start = 0, end = Infinity, step = 1) {
    let n = 0;
    for (let i = start; i < end; i += step) {
        n++;
        yield i;
    }
    return n;
}
```

### generator 함수 표현식 

```js
let createIterator = function *(items) {
    for (let i = 0; i < items.length; i++) {
        yield items[i];
    }
};
let iterator = createIterator([1, 2, 3]);
console.log(iterator.next());         // "{ value: 1, done: false }"
console.log(iterator.next());         // "{ value: 2, done: false }"
console.log(iterator.next());         // "{ value: 3, done: false }"
console.log(iterator.next());         // "{ value: undefined, done: true }"
// for all further calls
console.log(iterator.next());         // "{ value: undefined, done: true }"
```

- `createIterator()`는 함수 선언 대신에 *Generator* 함수 표현식입니다. 함수 표현식이 익명이므로 별표는 `function` 키워드와 여는 괄호 사이에 옵니다. 그 외에 이 예제는 `for` 루프를 사용했던 `createIterator()` 함수의 이전 버전과 동일합니다.
- arrow 함수로는 만들 수 없다..!

### generator 객체 메소드

- *Generator*는 함수이기 때문에 객체에도 추가할 수 있다!
- 함수 표현식을 사용하여 ECMAScript 5 스타일 객체 리터럴에서 *Generator*를 만들 수 있습니다.

```js
//ES5
var o = {
    createIterator: function *(items) {
        for (let i = 0; i < items.length; i++) {
            yield items[i];
        }
    }
};
let iterator = o.createIterator([1, 2, 3]);


//ES6
var o = {
    *createIterator(items) {
        for (let i = 0; i < items.length; i++) {
            yield items[i];
        }
    }
};
let iterator = o.createIterator([1, 2, 3]);
```





## 참고문헌 

- [MDN(번역함!)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Iterators_and_Generators)
- [ES6 Iterator & generator](https://infoscis.github.io/2018/01/31/ecmascript-6-iterators-and-generators/)
- [JS의 Iterator와 Generator](https://ui.toast.com/weekly-pick/ko_20151021/)
- [iterable protocol](https://medium.com/@pks2974/javascript%EC%99%80-iterator-cdee90b11c0f)
- [구조분석](https://medium.com/@madasamy/explanation-about-iterators-and-generators-in-javascript-es6-f7e669cbe96e)
- [구현 및 사용](https://velog.io/@victor/Javascript-iterator%EC%99%80-iterable-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%82%AC%EC%9A%A9%EB%B2%95)
- [사용 시 이점/장점](https://dev-momo.tistory.com/entry/Beginning-Javascript-Iterator-and-Generator)
- [symbol iterator](https://medium.com/@pks2974/javascript%EC%99%80-iterator-cdee90b11c0f)
