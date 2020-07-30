---
layout: post
title: "JS 33 concepts - 22_Higher Order Function"
author: "Oizys18"
categories: [Language]
comments: true
tags: [Javascript,JS33Concepts,Language,]
---

![img](https://camo.githubusercontent.com/339451b802bc713bd047d8396117023d652dcc49/68747470733a2f2f64657269636b6261696c65792e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031352f31302f756e7772617070696e672e6a706567)

## 고차함수란?

- 고차 함수는 함수 인수를 받거나 함수를 반환하는 함수입니다. ( === 함수를 주고받는 함수)
  - `map()`,`reduce()`,`filter()`,`setTimeout()`,`addEventListener()`,`bind()` ....

```js
// ES6 version
const characters =  [
	{name:  'Luke Skywalker', img:  'http://example.com/img/luke.jpg', species:  'human'},
	{name:  'Han Solo', img:  'http://example.com/img/han.jpg', species:  'human'},
	{name:  'Leia Organa', img:  'http://example.com/img/leia.jpg', species:  'human'},
	{name:  'Chewbacca', img:  'http://example.com/img/chewie.jpg', species:  'wookie'}
];

const humans = data => data.filter(character => character.species ===  'human');
const images = data => data.map(character => character.img);
const compose = (func1, func2) => data => func2(func1(data));
const displayCharacterImages = compose(humans, images);

console.log(displayCharacterImages(characters));

/* 
Logs out the following array
	["http://example.com/img/luke.jpg",
	 "http://example.com/img/han.jpg",
	 "http://example.com/img/leia.jpg"
	 ]
*/
```

- 함수의 빌딩블록을 최대한 기능단위로 잘게 쪼개면 테스트와 유지보수 비용을 줄이고 재사용성을 높일 수 있다.  

## 고차함수의 변수

### 함수 또한 변수이다.

- 함수는 특별한 변수입니다. 값을 다시 할당하여 다른 함수에 매개변수로 전달할 수 있습니다.
  (콜백 인자로 전달되는 비동기 함수를 작성하는데 유용합니다.)

### 변수의 스코프

- 함수 외부에서 정의 된 변수는 해당 함수로 접근 할 수 있습니다. 그것들은 함수 자체뿐만 아니라 그 함수 외부에서 수정 될 수 있습니다.

- 함수 내부에 정의 된 변수와 함수에 전달 된 이자는 함수 내에서만 접근이 가능합니다.

### 변수를 함수에 전달

- ​	변수가 함수에 인자로 전달되면 변수 값이 인자 안에 복사가 됩니다.

## [일급 클래스 함수](https://en.wikipedia.org/wiki/First-class_citizen)

- 자바스크립트가 함수를 [일급 시민](https://medium.com/@lazysoul/functional-programming-%EC%97%90%EC%84%9C-1%EA%B8%89-%EA%B0%9D%EC%B2%B4%EB%9E%80-ba1aeb048059)으로 취급한다고 들었을 것입니다. 이것이 의미하는 것은 자바스크립트의 함수가 객체로 취급된다는 것입니다. 자바스크립트는 객체 타입을 가지고, 변수의 값으로 할당될 수 있으며, 다른 참조 변수처럼 전달되고 반환될 수 있습니다.
- 1급 객체의 조건
  - 변수나 데이타에 할당 할 수 있어야 한다.
  - 객체의 인자로 넘길 수 있어야 한다.
  - 객체의 리턴값으로 리턴 할수 있어야 한다.





## Filter, Map, Reduce의 사용

- Filter,Map,Reduce는 모두 함수가 인수로 전달될 수 있다. 즉 고차함수다. 
- 일반적인 방법으로 써진 코드보다 반복을 줄이고 개선할 수 있다. 많이 활용하자
  - `forEach()`는 `side-effect`가 생성되어 `undefined`가 반환될 수가 있다. 함수형 프로그래밍을 하려한다면 지양하자.대신 이 세 함수를 사용하자! 

### 예시

18세 이하의 사람들을 걸러내기 위해 `.filter()`를 사용하는 것에 대해 이야기했습니다. `coffeeLover property`을 추가하는 `.map()`과 `reduce()`를 합쳐서 마침내 모든 사람의 나이를 합한 합계를 만듭니다. 실제로 이 세 단계를 결합한 코드를 작성해 봅시다.

```js
const people = [ 
  { name: ‘John Doe’, age: 16 }, 
  { name: ‘Thomas Calls’, age: 19 }, 
  { name: ‘Liam Smith’, age: 20 },
  { name: ‘Jessy Pinkman’, age: 18 }
];
const coffeeLovers = [‘John Doe’, ‘Liam Smith’, ‘Jessy Pinkman’];
const ageAbove18 = (person) => person.age >= 18;
const addCoffeeLoverProperty = (person) => { 
  person.coffeeLover = coffeeLovers.includes(person.name);  
  return person;
}
const ageReducer = (sum, person) => { return sum + person.age;}, 0);
const coffeeLoversAbove18 = people.filter(ageAbove18).map(addCoffeeLoverProperty);
const totalAgeOfCoffeeLoversAbove18 = coffeeLoversAbove18.reduce(ageReducer);
const totalAge = people.reduce(ageReducer);
```

### 예시2

cents를 나타내는 정수 값을 화폐로 형식화하는 작업이 있다고 가정하십시오. 요청에는 통화 기호 지정 및 소수 구분 기호와 같은 일부 사용자 정의가 포함됩니다.

`formatCurrency` 는 고정 통화 기호와 소수점 구분 기호가 있는 함수를 반환합니다.

```js
const formatCurrency = function( 
    currencySymbol,
    decimalSeparator  ) {
    return function( value ) {
        const wholePart = Math.trunc( value / 100 );
        let fractionalPart = value % 100;
        if ( fractionalPart < 10 ) {
            fractionalPart = '0' + fractionalPart;
        }
        return `${currencySymbol}${wholePart}${decimalSeparator}${fractionalPart}`;
    }
}

> getLabel = formatCurrency( '$', '.' );

> getLabel( 1999 )
"$19.99"

> getLabel( 2499 )
"$24.99"
```



## 비구조화 할당 `...`

- `비구조화 할당(destructuring assignment) 구문`은 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 `자바스크립트 표현식(expression)`

### 배열

```js
[a1, a2, ...rest_a] = [1, 2, 3, 4, 5, 6, 7, 8, 9];
console.log(a1); // 1
console.log(a2); // 2
console.log(rest_a); // [3, 4, 5, 6, 7, 8, 9]
```

- 좌항이 호출될 변수명 집합, 우항이 할당할 값 입니다.

  좌항의 각 요소에는 같은 index를 가지는 배열값이 할당됩니다.

  또한 전개 연산자( ... )를 사용하여 좌항에서 명시적으로 할당되지 않은 나머지 배열 값들을 사용할 수 있습니다.

```js
[a1, a2, ...rest_a, a3] = [1, 2, 3, 4, 5, 6, 7, 8, 9]; // error
[a1, a2, ...rest_a] = {a1 : 10, a2: 20}; // error
```

- 전개 연산자 이후에 변수를 입력하거나, 좌 우항이 다른 속성일 경우 에러가 발생합니다.

### 객체

```js
var { a1, a2, ...rest_a } = { a1 : 10, a2 : 20, a3 : 30, a4 : 40 };
console.log(a1); // 10
console.log(a2); // 20
console.log(rest_a); // { a3: 30, a4: 40 }
```

- 객체의 경우에는 우항의 key 값이 좌항의 변수명과 매칭됩니다.
- 배열과 마찬가지로 var, let, const가 적용 가능합니다.
- 또한 우항의 key값에 변수명으로 사용 불가능한 문자열이 있을경우 아래와 같은 방식으로 비구조화 할 수 있습니다.

```
var key = 'it is key';
var { 'an-apple':an_apple, [key]:it_is_key } = { 'an-apple' : 10, 'it is key' : 20};
console.log(an_apple); // 10
console.log(it_is_key); // 20
```

- 다만 이 경우에는 'an-apple'과 매칭할 변수명(an_apple)을 작성하지 않으면 에러가 발생하게 됩니다.

- 마지막으로 객체 비구조화에서 주의해야 할 점은 변수 선언에 대한 명시(var, let, const)가 없을 경우 괄호를 사용하여 묶어주어야 한다는 것 입니다.

```js
({ a, b } = { a : 10, b : 20});
console.log(a); // 10
console.log(b); // 20
{ c, d } = { c : 30, d : 40}; // error
```

### 함수에서 사용

함수의 파라미터 부분에서도 비구조화 할당을 사용할 수 있습니다.

이러한 문법은 특히 API 응답 값을 처리하는데에 유용하게 사용됩니다.

```js
function renderUser({name, age, addr}){
  console.log(name);
  console.log(age);
  console.log(addr);
}

const users = [
  {name: 'kim', age: 10, addr:'kor'},
  {name: 'joe', age: 20, addr:'usa'},
  {name: 'miko', age: 30, addr:'jp'}
];

users.map((user) => {
  renderUser(user);
});
```

마찬가지로 map함수의 파라미터에도 바로 사용할 수 있습니다.

```js
const users = [
  {name: 'kim', age: 10, addr:'kor'},
  {name: 'joe', age: 20, addr:'usa'},
  {name: 'miko', age: 30, addr:'jp'}
];

users.map(({name, age, addr}) => {
  console.log(name);
  console.log(age);
  console.log(addr);
});
```

### for of 문

뿐만 아니라 배열 내 객체들은 for of 문을 사용하여 비구조화 할 수 있습니다.

```js
const users = [
  {name: 'kim', age: 10, addr:'kor'},
  {name: 'joe', age: 20, addr:'usa'},
  {name: 'miko', age: 30, addr:'jp'}
];

for(var {name : n, age : a} of users){
  console.log(n);
  console.log(a);
}
```

### **중첩된 객체 및 배열의 비구조화** 

중첩된 객체 및 배열 역시 비구조화 할 수 있습니다.

```js
const kim = {
  name: 'kim',
  age: 10,
  addr: 'kor',
  friends: [
    {name: 'joe', age: 20, addr:'usa'},
    {name: 'miko', age: 30, addr:'jp'}
  ]
};

var { name: userName, friends: [ ,{ name: jpFriend }] } = kim;
console.log(userName); // kim
console.log(jpFriend); // miko
```

friends 배열의 호출 변수를 명시하는 대신, 배열을 한번 더 비구조화한다.

## 참고 및 추가공부

- [Functional Programming](https://www.youtube.com/playlist?list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84)
- [Map,Filter,Reduce](https://code.tutsplus.com/tutorials/how-to-use-map-filter-reduce-in-javascript--cms-26209)
- [JS Functional Programming](https://www.youtube.com/watch?v=e-5obm1G_FY)
- [Strings and Template Literals](http://www.zsoltnagy.eu/strings-and-template-literals-in-es6/)
- [JS의 `...`문법(비구조화 할당)](https://yuddomack.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%AC%B8%EB%B2%95-%EB%B9%84%EA%B5%AC%EC%A1%B0%ED%99%94-%ED%95%A0%EB%8B%B9)

