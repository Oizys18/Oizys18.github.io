---
layout: post
title: "JS 33 concepts - 27_DataStructure"
author: "Oizys18"
categories: [Language]
comments: true
tags: [Javascript, JS33Concepts, Language]
---
* TOC
{:toc}
* * *
![1594078403516](https://github.com/Oizys18/TIL/blob/master/images/02_Language/DataStructure.PNG?raw=true)

## Object와 Array

- 원시타입 6종류(Boolean, Null, Undefined, Number, String, Symbol) 이외의 데이터타입
- `array`도 `object`도 둘 다 `object`

```js
// arrays
[];

// objects
{
}
```

### [Object](https://developer.mozilla.org/ko/docs/Web/JavaScript/Data_structures)

- [식별자 (Identifier)](https://developer.mozilla.org/ko/docs/Glossary/식별자) 로 참조할 수 있는, 메모리에 있는 값
- 속성(Property)를 담고 있는 가방(Collection)
- 중괄호는 개체를 나타내며 개체의 요소가 개체를 구성합니다. 객체 값은 **키 : 값** 쌍으로 제공되며 이러한 값은 객체의 **속성** 또는 **방법 (함수)** 일 수 있습니다. 속성은 객체의 특징 또는 속성 인 반면, 방법은 객체에서 수행 할 수있는 기능 또는 동작입니다.

```js
// simple way to create a string
const myMessage = "look at me go!";

// 위의 코드를 JS는 아래와 같이 인식한다.
const myOtherMessage = String("look at me go!");

myMessage == myOtherMessage; // true
```

- Object property 접근

  - Dot notation

  ```js
  const objectName = {
    objectProp: "my super duper value",
  };

  objectName.objectProp; // my super duper value
  ```

  - Square Bracket Notation
    - 속성 이름이 대시나 숫자와 같은 특수 문자로 시작하거나 변수를 키로 사용하려는 경우 대괄호 표기법 만 사용할 수 있습니다.

  ```js
  const objectName = {
    objectProp: "super cool yo!",
  };

  objectName["objectProp"]; // super cool yo!
  ```

- Object property 순회
  - **for…in loop**: 이것은 가장 일반적인 것으로 객체의 열거 가능한 각 속성과 프로토 타입 체인을 반복하는 데 사용됩니다.
  - **Object.keys(o)**: object.keys (o) 메소드는 객체 o를 순회하는 데 사용되며 열거 가능한 모든 속성 이름이 포함 된 배열을 반환합니다.
  - **Object.getOwnPropertyNames(o)**: 덜 알려진 방법은 객체 o를 반복하고 열거 가능한 객체의 모든 자연 속성 이름 배열을 반환합니다.

### Array

```js
const arr = [];
const bar = ["drinks", "music", "dance", "beer", "more beer"];

// an array of objects
const coocoo = [
  { key: "thing", key2: 2 },
  { key3: "otherThing" },
  { key4: "my string" },
];

// new를 사용할 수 있음!
const bar = new Array("beer", "music", "beer", "more beer");
console.log(bar); // ['beer', 'music', 'beer', 'more beer']

// new 예약어 사용시 2개 이상의 인자를 주면 array로 만들어줌!
const fig = new Array(10, 20);
console.log(fig); // [10, 20]

// 1개만 사용하면 빈 array 10개를 줌!
const fig = new Array(10);
console.log(fig); // returns 10 empty arrays!
```

- `new` 키워드 사용하면 실수할 수도 있으니까 `[]` 리터럴 사용할 것이 권장된다.
- 객체와 달리 배열 값은 0부터 시작하는 색인 위치를 사용하여 액세스합니다
- **객체는 문자 인덱스를 사용하는 반면 배열은 숫자 인덱스를 사용합니다.**

- Array 조작

  - `pop()`: 배열에서 마지막 요소를 제거하고 해당 요소를 반환
  - `slice()`: 배열 부분의 얕은 복사본을 새로운 배열로 반환
  - `shift()`: 배열에서 첫 번째 요소를 제거하고 해당 요소를 반환
  - `unshift()`: 배열의 시작 부분에 하나 이상의 요소를 추가하고 배열의 새 길이를 반환

* **mutator 메서드**(열을 변경하거나 수정하는 메소드)

* `push()`: 지정된 요소를 배열에 추가하고 수정 된 배열을 반환

* **접근자(accessor) 메서드**(배열을 변경하지 않고 메소드의 효과에 따라 배열의 이미지를 작성하는 메소드) :

  - `slice()`: 배열 슬라이싱

* **반복(iteration) 메서드**(배열의 길이를 샘플링하고 메소드에 정의 된 콜백 함수로 배열의 각 요소를 평가하는 동안 배열을 반복하는 데 사용)

  - `map()`
  - `forEach()`

## 추상자료형 ADT(Abstract Data Type)

- **데이터의 추상적 형태**와 **그 데이터를 다루는 방법**만을 정해놓은 것

## [Stack](https://codepen.io/thonly/pen/GMyLOV)

- 데이터를 집어넣을 수 있는 선형(linear) 자료형
- LIFO
- 데이터를 집어넣는 push, 데이터를 추출하는 pop, 맨 나중에 집어넣은 데이터를 확인하는 peek
- 서로 관계가 있는 여러 작업을 연달아 수행하면서 **이전의 작업 내용을 저장해 둘 필요가 있을 때** 널리 사용됩니다.

```js
class Stack {
  constructor() {
    this._arr = [];
  }
  push(item) {
    this._arr.push(item);
  }
  pop() {
    return this._arr.pop();
  }
  peek() {
    return this._arr[this._arr.length - 1];
  }
}

const stack = new Stack();
stack.push(1);
stack.push(2);
stack.push(3);
stack.pop(); // 3
```

## [Queue](https://codepen.io/thonly/pen/KypxZg)

- 데이터를 집어넣을 수 있는 선형(linear) 자료형
- **배열** 또는 **링크드 리스트** 사용하여 큐를 구현할 수 있다.
- FIFO
- 데이터를 집어넣는 enqueue, 데이터를 추출하는 dequeue
- **순서대로 처리해야 하는 작업을 임시로 저장해두는 버퍼(buffer)**로서 많이 사용됩니다.
- `unshift`: 요소를 배열의 맨 앞에 추가한다.
- `pop`: 배열의 마지막 요소를 제거한다.

```js
class Queue {
  constructor() {
    this._arr = [];
  }
  enqueue(item) {
    this._arr.push(item);
  }
  dequeue() {
    return this._arr.shift();
  }
}

const queue = new Queue();
queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);
queue.dequeue(); // 1
```

## [Linked List](https://codepen.io/thonly/pen/QqRVJX)

- 연결된 목록은 클라이언트와 서버 모두에 유용합니다. 클라이언트에서 Redux와 같은 상태 관리 라이브러리는 미들웨어 논리를 연결 목록 방식으로 구성합니다. 조치가 전달되면 리듀서에 도달하기 전에 모든 것이 방문 될 때까지 미들웨어에서 다음 미들웨어로 파이프됩니다. 서버에서 Express와 같은 웹 프레임 워크도 비슷한 방식으로 미들웨어 논리를 구성합니다. 요청이 수신되면 응답이 발행 될 때까지 하나의 미들웨어에서 다음 미들웨어로 파이프됩니다.

```js
var LinkedList = function () {
  var list = {};
  list.head = null;
  list.tail = null;

  list.addToTail = function (value) {
    let node = new Node(value);

    if (!this.head) {
      this.head = node;
      this.tail = node;
    } else {
      this.tail.next = node;
      this.tail = node;
    }
  };

  list.removeHead = function () {
    let removeNode = this.head;
    if (TimeRanges.head !== null) {
      this.head = removeNode.next;
      return removeNode.value;
    }
  };

  list.contains = function (target) {
    let accNode = this.head;
    while (accNode) {
      if (accNode.value === target) {
        return true;
      }
      accNode = accNode.next;
    }
    return false;
  };

  return list;
};

var Node = function (value) {
  var node = {};

  node.value = value;
  node.next = null;

  return node;
};
```

## [Tree](https://codepen.io/thonly/pen/qVWOpM)

```js
var Tree = function (value) {
  var newTree = Object.create(treeMethods);
  newTree.value = value;

  newTree.children = [];

  return newTree;
};

var treeMethods = {};

treeMethods.addChild = function (value) {
  let node = new Tree(value);
  this.children.push(node);
};

treeMethods.contains = function (target) {
  let result = false;

  function recusion(element) {
    if (element.value === target) {
      result = true;
      return;
    }
    for (let i = 0; i < element.children.length; i++) {
      recusion(element.children[i]);
    }
  }
  recusion(this);
  return result;
};
```

## [Graph](https://www.zerocho.com/category/Algorithm/post/584b9033580277001862f16c)

- 트리에 둘 이상의 부모가있는 경우 그래프
- 그래프에서 노드를 함께 연결하는 에지는 방향을 지정하거나 방향을 지정하지 않거나 가중치를 적용하거나 가중치를 지정할 수 없습니다. 방향과 무게가 모두있는 모서리는 벡터와 유사합니다. Mixin 형태의 다중 상속과 다 대다 관계가있는 데이터 객체는 그래프 구조를 생성합니다.

## [Hash table](https://codepen.io/thonly/pen/bYpQar)

- 해시 테이블은 키와 값을 쌍으로하는 사전과 유사한 구조
- 각 쌍의 메모리 위치는 키를 받아들이고 쌍을 삽입하고 검색해야하는 주소를 반환하는 해시 함수에 의해 결정
- 복잡한 주소 매핑을 위해 맵 또는 객체를 사용할 수 있다
- 해시 테이블은 평균적으로 일정한 시간을 삽입하고 조회한다. 충돌과 크기 조정으로 인해이 무시할만한 비용은 선형 시간으로 증가 할 수 있다
- 데이터베이스 쿼리 속도는 레코드를 가리키는 인덱스 테이블을 정렬 된 순서로 유지하는 데 크게 의존합니다. 이런 식으로 이진 검색을 로그 시간으로 수행 할 수 있으며, 특히 빅 데이터의 경우 성능이 크게 향상됩니다.
- Reselect 라이브러리는이 캐싱 전략을 사용하여 Redux 지원 애플리케이션에서 mapStateToProps 기능을 최적화합니다. 실제로 JavaScript 엔진은 heaps라는 해시 테이블을 사용하여 우리가 만든 모든 변수와 프리미티브를 저장합니다.

```js
var HashTable = function () {
  this._limit = 8;
  this._storage = LimitedArray(this._limit);
};

HashTable.prototype.insert = function (k, v) {
  var index = getIndexBelowMaxForKey(k, this._limit);
  let value = this._storage.get(index);
  let obj = {};
  if (value) {
    obj = value;
    obj[k] = v;
    this._storage.set(index, obj);
  } else {
    obj[k] = v;
    this._storage.set(index, obj);
  }
};

HashTable.prototype.retrieve = function (k) {
  var index = getIndexBelowMaxForKey(k, this._limit);
  return this._storage.get(index)[k];
};

HashTable.prototype.remove = function (k) {
  var index = getIndexBelowMaxForKey(k, this._limit);
  this._storage.each(function (el, i) {
    if (index === i) {
      delete el[k];
    }
  });
};
```

## 참고

- [JS로 만나는 세상](https://helloworldjavascript.net/pages/282-data-structures.html)
- [JS 알고리즘](https://github.com/trekhleb/javascript-algorithms)
- [JS linked list](https://velog.io/@760kry/JS-Linked-List-vs-Array-List-data-structor)
- [자료구조](https://m.blog.naver.com/PostView.nhn?blogId=islove8587&logNo=220548856458&proxyReferer=https:%2F%2Fwww.google.com%2F)
