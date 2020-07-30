---
layout: post
title: "JS 33 concepts - 24_Collection and Constructor"
author: "Oizys18"
categories: [Language]
comments: true
tags: [Javascript,JS33Concepts,Language,]
---

## [컬렉션](https://velog.io/@yesdoing/JavaScript-Collections) Collection

- 프로그래밍 언어가 제공하는 값을 담을 수 있는 컨테이너
  - Python - list, tuple, dictionaries ...
  - Java - ArrayList, HashMap, HashSet, Queue, Stack ...
  - Ruby - hashes, arrays
  - Javascript(기존 Arrays + Objects, 이후 ES6에서 새로 추가)
    - Indexed Collection - Arrays, `Typed Array`
    - Keyed Collection - Objects, `Map`, `Set`, `Weak Map`, `Weak Set`

# Map

- Key - Value 의 쌍으로 이루어진 컬렉션
- 모든 유형의 데이터를 키로 사용할 수 있다.
- 키-값 쌍을 추가하려면 `.set()` 메소드를 사용한다.
- `new`를 통해 인스턴스화 할 수 있다.

### 메소드와 프로퍼티

- `new Map()` – 맵을 생성한다.
- `map.set(key, value)` – key와 value값을 저장한다.
- `map.get(key)` – key가 map에 없는 경우 undefined로 값을 리턴한다.
- `map.has(key)` – key가 있으면 true를 반환하고 그렇지 않으면 false를 리턴한다.
- `map.delete(key)` – key값을 삭제한다.
- `map.clear()` – map을 돌며 모두 삭제한다.
- `map.size` – 최근 사용된 엘리먼트의 갯수를 리턴한다.
- map에서 루프돌기 (Iteration)
  - `map.keys()` – key를 리턴한다.
  - `map.values()` – value를 리턴한다.
  - `map.entries()` – [key, value] 항목에 대해 허용 가능한 값을 리턴한다. 이 값은 기본적으로 `for..of`에서 사용된다.

### 일반 객체의 문제점

- 객체의 키로 내장 메소드의 이름을 사용할 시 이름 충돌이 일어날 수 있습니다.
- 속성의 Key는 항상 문자열 이어야 합니다. (ES6 에서는 심볼도 가능합니다.)
- 객체에 얼마나 많은 속성이 존재하는지 알아낼 수 있는 효과적인 방법이 없습니다.(`size`나 `length` 같은 메서드가 없습니다.)
- 일반 객체를 반복하려면 많은 비용이 소모됩니다.

여기서 가장 큰 문제점은 일반 객체는 `iterable` 하지 않기 때문에 `iterable`을 사용하는 모든 문법에 객체를 사용할 수 없습니다.

하지만 직면한 문제가 위의 기능들을 필요로 하지 않는다면, 일반 객체를 사용하는 것이 올바른 선택일 수 있습니다.

### 주의

- ES6 컬렉션이 사용자 데이터와 빌트인 메소드 사이의 이름 충돌을 피하기 위해 설계 되었기 때문에, **ES6 컬렉션은 자신의 멤버 데이터를 드러내기 위해 속성(property)를 사용하지 않습니다.**
- 이로 인해 해시 테이블의 멤버 데이터에 접근하기 위해 `obj.key` 또는 `obj[key]` 같은 구문을 사용할 수 없습니다. **객체의 값을 가져오기 위해서는 `map.get(key)` 구문을 이용해야하며 해시 테이블의 멤버 데이터는 속성과 달리 프로퍼티 체인(property chain)을 통해 상속되지 않습니다.**
- `map`은 키를 비교할 때 동등성을 테스트하기 위해 [SameValueZero](https://tc39.es/ecma262/#sec-samevaluezero) 알고리즘을 사용한다. 엄격한 평등 `===` 과 동일하지만, 차이점은 `NaN`이 `NaN`과 동등하다고 여겨진다는 것이다. 그래서 `NaN`도 key로 활용할 수 있다. 이 알고리즘은 변경하거나 사용자 정의할 수 없다.

### 사용 예시

```js
const myMap = new Map; // new 생성자로 선언해서 사용할 수 있습니다.
myMap.set('yesdoing', 'looser');

const person = { age: '111', gender: 'none', name: 'yesdoing'};
const whoami = {};

myMap.set(person, whoami); // 객체를 키와 값으로 받을 수 있습니다.

for(const [key, value] of myMap) { // destructuring으로 값을 가져와서 쓸 수 있어요.
	console.log(`${key} = ${value}`);  // yesdoing = looser [object Object] = [object Object]
}

myMap.get(person); // {}

myMap.forEach(value, key) => { // 다른 forEach와의 호완을 위해서 value, key 순입니다.
	console.log(`${key} = ${value}`);  // yesdoing = looser [object Object] = [object Object]	
}, myMap);

myMap.clear(); // map에 있는 엔트리를 전부 삭제합니다. 
console.log(myMap.size); // 0

// 모든 map.set은 map 자체를 반환하므로, 다음 호출을 "체인"할 수 있다.
map.set('1', 'str1')
  .set(1, 'num1')
  .set(true, 'bool1');

// [[키,값]] 의 쌍으로 map을 생성가능
let map = new Map([
  ['1',  'str1'],
  [1,    'num1'],
  [true, 'bool1']
]);

// Object.fromEntries는 [key, value] 쌍의 배열을 지정하면 다음과 같은 방법으로 객체를 생성한다.
let prices = Object.fromEntries([
  ['banana', 1],
  ['orange', 2],
  ['meat', 4]
]);
```

# Set

- Set은 value를 키 값으로 갖는 컬렉션입니다.
- Set은 수정 가능하며, 프로그램이 실행되는 동안 값의 추가나 삭제를 할 수 있습니다.
- 각 값이 한 번만 발생할 수 있는 특수 유형 컬렉션 
  - key가 존재하지 않는다.

### 속성과 메소드

- `new set(iterable)` – 세트를 작성하고, 허용 가능한 개체(일반적으로 배열)가 제공된 경우, 해당 개체에서 세트로 값을 복사한다.
- `set.add(value)` – 값을 추가하고 set 자체를 반환한다.
- `set.delete(value)` – 값을 삭제하고, 호출 순간에 값이 존재하면 true를 반환하고, 그렇지 않으면 false를 반환한다.
- `set.has(value)` – 값이 set에 있으면 true를 반환하고 그렇지 않으면 false를 반환한다.
- `set.clear()` – 집합에서 모든 것을 제거한다.
- `set.size` – 요소 개수.
- set에서 루프돌기 (Iteration)
  -  `for..of` 혹은 `forEach`
  - `set.keys()` – 값에 대해 허용 가능한 개체를 반환
  - `set.values()` – set.keys()와 동일하며, 지도와의 호환성
  - `set.entries()` – [value, value] 항목에 대해 허용 가능한 개체를 반환하여 map과 호환

### 값이 중복되지 않습니다.

- 이미 존재하는 값을 추가하려고 하면 아무 일도 일어나지 않습니다.

  const mySet = new Set("abcd");
  mySet.size; // 4
  mySet.add("a");
  mySet.size; // 4

### Set은 뚜렷한 목적을 가지고 데이터를 관리합니다.

바로 어떤 데이터가 자신의 멤버인지 확인하는 작업을 빨리 처리하려는 목적입니다.

```js
const mySet = new Set("abcd");
const myArray = [..."abcd"];

myArray.indexOf("a") !== -1 // true slow
mySet.has("a")              // true fast
```

### Set으로 할 수 없는 일

- Set은 인덱스 값으로 데이터를 조회하는 일을 할 수 없습니다.

```js
myArray[0]; // "a"
mySet[0];   // undefined
```

### Set 메소드 사용예시

```js
let mySet = new Set;  // 그냥 new 생성자로 빈 Set을 선언할 수 있습니다.
let iterSet = new Set([1, 2, 3]); // 생성할때 iterable한 객체를 초기 값으로 줄 수 있습니다.
console.log(iterSet.size);  // size로 Set의 크기를 알 수 있습니다.
console.log(iterSet.has(1)); // has로 Set에 값이 존재하는지 알 수 있습니다.
mySet.add(1).add(2).add(3); // add로 Set에 값을 추가합니다. 이것은 체이닝 될 수 있습니다.
mySet.delete(1).delete(2);  // delete로 Set에 값을 삭제할 수 있습니다. 역시 체이닝 될 수 있습니다. 
iterSet.forEach((value, value, iter) => v); // Array.prototype.forEach 와 같은 forEach를 제공합니다. 
iterSet.clear(); // Set의 모든 값을 삭제합니다.
```

### Set에서 구현되지 않은 것.

- Array.prototype에 구현된 `map`, `filter`, `some`, `every`등의 내장함수 미구현
- 많은 값들을 한번에 처리할 수 있는 메소드들이 누락 (Ex. `set.addAll(iterable)`, `set.removeAll(iterable)` 등등
- Set에 Object 값이 들어오는 경우 레퍼런스(주소 값) 이 다르면 값이 같아도 다르게 처리됩니다.

물론 이것들은 ES6의 다른 문법들을 통해서 구현 가능합니다.

# Weak Collections

Map과 Set이 참조하는 객체들은 강하게 연결되어 있습니다. 이 것은 JavaScript의 가비지 컬렉션이 메모리 수거를 못하게 막는 원인이 됩니다. 만약 크기가 큰 Map과 Set의 객체가 더 이상 쓰이지 않는다면 가비지 컬렉션에서 이것을 가져가기 위해 비싼 비용을 치뤄야 합니다.

이것을 해결하기 위해 ES6에서는 Weak Map, Weak Set이 나왔습니다.

이 컬렉션들은 더 이상 사용되지 않을 때, 메모리에서 쉽게 삭제 되기 위해 '약한' 결합을 유지합니다.

### Weak Map

Weak Map은 Map과 비슷합니다. 단지 메서드가 몇개 없고, 가비지 컬렉션의 처리가 다릅니다.

```js
const yesdoing = new WeakMap(); // WeakMap을 생성합니다. 
const age = {}; // 키는 반드시 객체여야 합니다.
const job = {}; // 키는 반드시 객체여야 합니다.

yesdoing.set(age, 11111); // 키 - 값을 설정합니다.
yesdoing.set(job, 'air'); // 값으로는 어떤 타입이라도 들어올 수 있습니다. 

yesdoing.has(job); // True
yesdoing.delete(job) // key를 삭제합니다. 

_makeClone() {
  this._containerClone = this.container.cloneNode(true);
  this._cloneToNodes = new WeakMap();
  this._nodesToClones = new WeakMap();

  ...

  let n = this.container;
  let c = this._containerClone;

  // find the currentNode's clone
  while (n !== null) {
    if (n === this.currentNode) {
    this._currentNodeClone = c;
    }
    this._cloneToNodes.set(c, n);
    this._nodesToClones.set(n, c);

    n = iterator.nextNode();
    c = cloneIterator.nextNode();
  }
```

### Weak Set

Weak Set 역시 Set과 비슷하지만 메서드가 몇개 없습니다.

```js
const yesdoing = new WeakSet(); // WeakMap을 생성합니다. 
const age = {}; // 값은 반드시 객체여야 합니다. 

yesdoing.add(age); // 값을 추가합니다.

yesdoing.has(age); // True
yesdoing.delete(age) // 값을 삭제합니다

let isMarked     = new WeakSet()
let attachedData = new WeakMap()

export class Node {
    constructor (id)   { this.id = id                  }
    mark        ()     { isMarked.add(this)            }
    unmark      ()     { isMarked.delete(this)         }
    marked      ()     { return isMarked.has(this)     }
    set data    (data) { attachedData.set(this, data)  }
    get data    ()     { return attachedData.get(this) }
}

let foo = new Node("foo")

JSON.stringify(foo) === '{"id":"foo"}'
foo.mark()
foo.data = "bar"
foo.data === "bar"
JSON.stringify(foo) === '{"id":"foo"}'

isMarked.has(foo)     === true
attachedData.has(foo) === true
foo = null  /* remove only reference to foo */
attachedData.has(foo) === false
isMarked.has(foo)     === false
```