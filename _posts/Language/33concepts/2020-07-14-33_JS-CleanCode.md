---
layout: post
title: "JS 33 concepts - 33_JS 클린코드"
author: "Oizys18"
categories: [Language]
comments: true
tags: [Javascript,JS33Concepts,Language]
---


- 최소기능단위로 함수를 분리할 것 (하나의 함수는 한가지 일만 한다.)

- 변수/함수명은 수행하는 작업을 나타내는 이름으로 짓는다. 

- If / Switch 문을 회피하자. 

  - 대부분의 조건문은 별도의 함수와 클래스로 대신할 수 있다. 

- 읽기 쉽고 추측의 여지가 없는 코드를 작성하자. 

- [SOLID 원칙](http://www.nextree.co.kr/p6960/) 이해 및 적용

  - ### 단일책임의 원칙: Single Responsibility Principle

  - ### 개방폐쇄의 원칙: Open Close Principle

  - ### 리스코브 치환의 원칙: The Liskov Substitution Principle

  - ### 인터페이스 분리의 원칙: Interface Segregation Principle

  - ### 의존성역전의 원칙: Dependency Inversion Principle

## 변수명  잘 짓는 법

```js
// DON'T
let d
let elapsed
const ages = arr.map((i) => i.age)

// DO
let daysSinceModification
const agesOfUsers = users.map((user) => user.age)
```

- Intention-revealing names 
  - 기능/내용을 나타내는 이름으로 작성
  - 검색 용이성, 코드가독성 

```js
// DON'T
let nameString
let theUsers

// DO
let name
let users
```

- 불필요한 명사 추가하지 않기

```js
// DON'T
let fName, lName
let cntr

let full = false
if (cart.size > 100) {
  full = true
}

// DO
let firstName, lastName
let counter

const MAX_CART_SIZE = 100
// ...
const isFull = cart.size > MAX_CART_SIZE
```

- 발음하기 쉬운 변수명 사용하기 

## 함수작성법

```js
// DON'T
function getUserRouteHandler (req, res) {
  const { userId } = req.params
  // inline SQL query
  knex('user')
    .where({ id: userId })
    .first()
    .then((user) => res.json(user))
}

// DO
// User model (eg. models/user.js)
const tableName = 'user'
const User = {
  getOne (userId) {
    return knex(tableName)
      .where({ id: userId })
      .first()
  }
}

// route handler (eg. server/routes/user/get.js)
function getUserRouteHandler (req, res) {
  const { userId } = req.params
  User.getOne(userId)
    .then((user) => res.json(user))
}
```

- *Functions should do one thing. They should do it well. They should do it only. — Robert C. Martin (Uncle Bob)*
- 한번에 한가지 기능만을 가진 함수를 작성할 것 (기능 단위로 분리할것)

```js
// DON'T
/**
 * Invite a new user with its email address
 * @param {String} user email address
 */
function inv (user) { /* implementation */ }

// DO
function inviteUser (emailAddress) { /* implementation */ }
```

- 함수명은 어떤 기능을 하는 지 바로 알 수 있어야 한다. (주석없이도!) 

### 긴 인자 리스트 피하기 

```js
// DON'T
function getRegisteredUsers (fields, include, fromDate, toDate) { /* implementation */ }
getRegisteredUsers(['firstName', 'lastName', 'email'], ['invitedUsers'], '2016-09-26', '2016-12-13')

// DO
function getRegisteredUsers ({ fields, include, fromDate, toDate }) { /* implementation */ }
getRegisteredUsers({
  fields: ['firstName', 'lastName', 'email'],
  include: ['invitedUsers'],
  fromDate: '2016-09-26',
  toDate: '2016-12-13'
})
```

- 객체 파라미터를 사용하기

### 사이드 이펙트 줄이기

```js
// DON'T
function addItemToCart (cart, item, quantity = 1) {
  const alreadyInCart = cart.get(item.id) || 0
  cart.set(item.id, alreadyInCart + quantity)
  return cart
}

// DO
// not modifying the original cart
function addItemToCart (cart, item, quantity = 1) {
  const cartCopy = new Map(cart)
  const alreadyInCart = cartCopy.get(item.id) || 0
  cartCopy.set(item.id, alreadyInCart + quantity)
  return cartCopy
}

// or by invert the method location
// you can expect that the original object will be mutated
// addItemToCart(cart, item, quantity) -> cart.addItem(item, quantity)
const cart = new Map()
Object.assign(cart, {
  addItem (item, quantity = 1) {
    const alreadyInCart = this.get(item.id) || 0
    this.set(item.id, alreadyInCart + quantity)
    return this
  }
})
```

- 순수함수를 사용하십시오!

### stepdown rule에 따라 함수를 정리할 것 

```js
// DON'T
// "I need the full name for something..."
function getFullName (user) {
  return `${user.firstName} ${user.lastName}`
}

function renderEmailTemplate (user) {
  // "oh, here"
  const fullName = getFullName(user)
  return `Dear ${fullName}, ...`
}

// DO
function renderEmailTemplate (user) {
  // "I need the full name of the user"
  const fullName = getFullName(user)
  return `Dear ${fullName}, ...`
}

// "I use this for the email template rendering"
function getFullName (user) {
  return `${user.firstName} ${user.lastName}`
}
```

- 고차함수는 위에 있어야한다..

### Query 혹은 Modification

- 함수는 뭔가를 하거나 (modify) 뭔가를 반환하거나 (query) 둘 중 하나여야 한다. 동시에 둘 다 하면 안됨 

### async code 잘 쓰기 

```js
// AVOID
asyncFunc1((err, result1) => {
  asyncFunc2(result1, (err, result2) => {
    asyncFunc3(result2, (err, result3) => {
      console.lor(result3)
    })
  })
})

// PREFER
asyncFuncPromise1()
  .then(asyncFuncPromise2)
  .then(asyncFuncPromise3)
  .then((result) => console.log(result))
  .catch((err) => console.error(err))
```

- nested callback 대신 프로미스 콜 사용합시다 

### const/let 관련

```js
console.log(usingVar); // undefined
var usingVar = "defined";
console.log(usingVar); // "defined"

console.log(usingLet); // error
let usingLet = "defined"; // never gets executed
console.log(usingLet); // never gets executed
```

- let이 작성되기전에 부르면 이후에 입력도 안됨!(실행이 안됨!)
- 최신 JavaScript에서 `var`를 가장 깨끗하게 드롭 인 대체 할 수 있습니다. 그러나 `var`와 구별되는 몇 가지 특성이 있습니다. `var`를 사용하는 변수 선언은 해당 스코프 내부에 배치 된 위치에 관계없이 기본적으로 포함된 스코프의 상단에 항상 올려져 있습니다. 즉, 깊이 중첩 된 변수조차도 포함된 스코프의 시작 부분에서 선언되고 사용 가능한 것으로 간주 될 수 있습니다. `let` 또는 `const`도 마찬가지입니다.
- `let` 또는 `const`를 사용하여 변수를 선언하면 해당 변수의 스코프는 선언 된 로컬 블록으로 제한됩니다. JavaScript의 블록은 함수 본문 또는 루프 내의 실행 코드와 같은 중괄호 `{}`로 구분됩니다.

### 화살표함수

```js
const traditional = function(data) {
  return (`${data} from a traditional function`);
}
const arrow = data => `${data} from an arrow function`;

console.log(traditional("Hi")); // "Hi from a traditional function"
console.log(arrow("Hi"));  // "Hi from an arrow function"
```

- 화살표 함수는 여러 가지 특성을 캡슐화하여 더 편리하게 부를 수 있으며, 함수를 부를 때 유용하지 않은 다른 행동을 배제합니다. 보다 다양한 기존 `function`키워드를 대체하는 것은 아닙니다.
- 예를 들어, 화살표 함수는 `this`와 `arguments`를 호출 된 컨텍스트에서 상속합니다. 프로그래머가 요청 된 컨텍스트에 적용하기 위해 호출되는 동작을 자주 원할 때 이벤트 처리 또는 `setTimeout`과 같은 상황에 유용합니다. 기존 함수는 프로그래머가 `.bind(this)`를 사용하여 기존 `this`에 함수를 바인딩하는 복잡한 코드를 작성하도록 강요했습니다. 화살표 함수에는 필요하지 않습니다.