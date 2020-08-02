---
layout: post
title: "JS 33 concepts - 17_프로토타입의 상속과 체인"
author: "Oizys18"
categories: [Language]
comments: true
tags: [Javascript,JS33Concepts,Language,]
---
## 목차
* TOC
{:toc}
* * *
![img](https://camo.githubusercontent.com/666ddb2149ddfaadf7d80c5c344b8cdfc8495529/68747470733a2f2f70726f636573732e66696c65737461636b6170692e636f6d2f63616368653d6578706972793a6d61782f726573697a653d77696474683a313035302f344a4839685230365132536d31467466376b6661)

## prototype vs Prototype

1. prototype (속성) - **특정 객체의 prototype 객체**: JavaScript로 작성한 모든 기능의 속성으로 지정된 특수 객체입니다. 여기서는 분명히 설명 하겠지만, JavaScript가 제공하는 내부 함수 (및`bind`에 의해 반환 된 함수)에는 필수 항목이 아닌 모든 함수에 대해 이미 존재합니다. 이`prototype`은 (`new` 키워드를 사용하여) 그 함수에서 새로 생성 된 객체의`[[Prototype]]`(아래 참조)이 가리키는 것과 동일한 객체입니다.
2. [[Prototype]] (링크속성) - **생성자 함수의 prototype 프로퍼티가 가리키는 prototype 객체**: 이것은 객체에서 읽혀지는 일부 속성을 사용할 수 없는 경우 실행중인 컨텍스트가 액세스하는 모든 객체의 숨겨진 속성입니다. 이 속성은 단순히 객체가 만들어진 함수의 `프로토 타입` 에 대한 참조입니다. 스크립트에는 **getter-setter** (다른 날 주제)라는 __proto__를 사용하여 액세스 할 수 있습니다.이 프로토 타입에 액세스하는 다른 새로운 방법이 있지만 간단히하기 위해 `[[Prototype]]` `__proto__`을 사용합니다. (` __proto__ == [[Prototype]] 링크` )

## String, Number, Boolean 프로토타입

- 순수 원시값 ( [[PrimitiveValue]] ) 은 prototype 이 존재하지 않는다.
- 하지만 원시값을 읽을 때 래퍼 타입이 생성되어, 해당 객체의 prototype 을 사용할 수 있다.
- 원시값을 호출시 래퍼 타입 객체를 생성하고 읽은 후에는 래퍼 타입 객체를 제거한다.
- new 연산자를 이용해 참조타입으로 생성하면, 제거하지 않고 계속 유지한다.

```js
var number = 123;
var string = 'string';
var boolean = true;

(123).__proto__ === Number.prototype
'string'.__proto__ === String.prototype
true.__proto__ === Boolean.prototype
```



## Prototype Chain 

- Javascript 에서 객체의 프로퍼티 나 메서드 에 접근하려고 할때, 객체에 존재 하지 않는다면, [[Prototype]] 링크를 따라 차례대로 부모 prototype 객체를 검색하며, 종료 prototype 객체까지 없을때는 undefined 를 반환한다.

- 거의 모든 객체는 Object.prototype 을 상속하기 때문에, Object.prototype 에 추가되어 있는 메서드를 사용할 수 있다.



![img](https://github.com/Lee-hyuna/33-js-concepts-kr/wiki/resource/yongkwan/17/01.png)

- 객체는 다른 객체를 참조하는 숨겨진 속성인 [[prototype]]을 갖고있다.

![img](https://github.com/Lee-hyuna/33-js-concepts-kr/wiki/resource/yongkwan/17/04.png)

- 길어질 수도 있다.

- 각 객체에는 프로토 타입이라고 하는 다른 객체에 대한 링크를 보유하는 private 속성( [Prototype](https://github.com/Lee-hyuna/33-js-concepts-kr/wiki/Prototype)이라고 함 )이 있습니다. 프로토 타입 객체는 자체 프로토 타입을 가지고 있으며 객체가 프로토 타입으로 null로 도달할 때까지 계속 진행됩니다. 정의에 따르면 정의에 따르면 null에는 프로토 타입이 없으며이 프로토 타입 체인의 최종 링크 역할을합니다.
-  `object`에서 속성 값을 접근하려고 할때, 해당 값이 존재하지 않으면 자바스크립트는 자동적으로 그 객체의 프로토타입(원형)에서 그 값을 가져온다.
- **`__proto__` 는 `[[Prototype]]`을 위한 historical getter/setter이다.**
- `__proto__`와 `[[Prototype]]`과 _같지 않다_는 것을 기억해라. `__proto__`는 `[[Prototype]]`의 getter/setter이다.
- `__proto__`는 역사적 이유로 존재하는데, 현대 언어에서는 `Object.getPrototypeOf/Object.setPrototypeOf` 같은 함수 형태로 대체되었다. 
- 스펙에 따르면 `__proto__`는 브라우저에 의해서만 지원되어야 하지만, 사실 서버 측을 비롯한 모든 환경에서 지원한다. 



### 제약사항

1. 참조는 순환이 되어선 안된다. `__proto__`를 순환하도록 할당하면 자바스크립트는 오류를 던질 것이다.
2. `__proto__`의 값은 객체이거나 `null` 이어야한다. 원시 값과 같은 다른 타입들은 무시된다.
3. `[[Prototype]]`은 오직 한개만 가질 수 있다. 한 객체는 두개의 객체에서 동시에 상속 받을 수 없다.

## Object.prototype

```js
var obj1 = Object.create(null);
console.dir(obj1);

var obj2 = {};
console.dir(obj2);
// Object
//  __proto__: Object
Object.setPrototypeOf(obj2, null);
console.dir(b);
// Object
//  No properties
```

- JavaScript 에서 거의 모든 객체는 Object의 인스턴스이다.
- 객체는 Object.prototype 에서 속성과 메서드를 상속받는다.
- 만약 프로토타입을 초기화 하고 싶다면, Object.create 와 Object.setPrototypeOf 메서드를 이용하여 프로토타입을 초기화 하거나 변경 할 수 있다.



## 호출함수를 통한 상속

- `call`함수는 기본적으로 다른 곳에서 정의된 함수를 호출 할 수 있지만 현재 컨텍스트에서는 허용하지 않습니다. 
- 첫번째 매개 변수는 함수를 실행할 때 사용할 값을 지정하며, 다른 매개 변수는 호출 될 때 함수에 전달 되어야하는 매개 변수입니다.

## for ... in loop

```js
let animal = {
  eats: true
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

// Object.keys only return own keys
alert(Object.keys(rabbit)); // jumps

// for..in loops over both own and inherited keys
for(let prop in rabbit) alert(prop); // jumps, then eats
```

원하는 결과가 이것이 아닐 것이다. 상속 받은 속성을 제외하고 싶을 것이다. 이를 위한 내장 메서드 [obj.hasOwnProperty(key)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)가 있다. 이 메서드는 속성 이름 `key`가 `obj`가 가진 속성이면 (상속 받은 것이 아니라), `true`를 반환한다.

따라서 상속된 속성들을 걸러낼 수 있다.

```js
let animal = {
  eats: true
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

for(let prop in rabbit) {
  let isOwn = rabbit.hasOwnProperty(prop);

  if (isOwn) {
    alert(`Our: ${prop}`); // Our: jumps
  } else {
    alert(`Inherited: ${prop}`); // Inherited: eats
  }
}
```

## 요약

- 자바스크립트에서, 모든 객체는 숨겨진 `[[Prototype]]`속성을 갖는다. 그리고 이 속성은 또다른 객체이거나 `null`이다.
- `obj.__proto__`를 사용하여 이 속성에 접근할 수 있다.
- `[[Prototype]]`에 의해 참조되는 객체를 "프로토타입(원형)"이라고 한다.
- `obj`의 속성을 읽거나 메서드를 호출하고 싶은데 존재하지 않는다면, 자바스크립트는 프로토타입에서 그것을 찾으려 할 것이다.
- 읽기/지우기 연산은 객체에 직접적으로 적용된다. 프로토타입을 사용하지 않는다.(setter가 아닌 데이터 속성이라 가정했을 경우).
- `obj.method()`를 호출하고, 해당 `method`가 프로토타입에서 가져온 것이라면, `this`는 여전히 `obj`를 킨다. 심지어 메소드들이 상속됬다 하더라도 메소드들을 언제나 현재 객체에 적용될 것입니다.
- `for..in` 루프는 자신의 값과 상속된 값을 반복한다. 그 외의 key/value를 가져오는 다른 모든 메소드들은 오직 그 자신의 객체의 속성만 가져온다.





## 참고

- [Javascript 와 Prototype 프로토 타입]:https://medium.com/@pks2974/javascript-%EC%99%80-prototype-%ED%94%84%EB%A1%9C%ED%86%A0-%ED%83%80%EC%9E%85-515f759bff79(https://medium.com/@pks2974/javascript-와-prototype-프로토-타입-515f759bff79)