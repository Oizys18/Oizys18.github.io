---
layout: post
title: "JS 33 concepts - 14_팩토리와 클래스"
author: "Oizys18"
categories: [Language]
comments: true
tags: [Javascript,JS33Concepts,Language,]
---

- JavaScript는 프로토타입 기반 언어이며, JavaScript의 모든 객체에는 `[[Prototype]]`이라는 숨겨진 내부 속성이 있어 객체 특성 및 메서드를 확장하는 데 사용할 수 있습니다.

  -  자바스크립트에는 클래스라는 개념이 없다. 대신 프로토타입(Prototype)이라는 것이 존재한다. 자바스크립트가 프로토타입 기반 언어라고 불리는 이유이다.

    클래스가 없으니 기본적으로 상속기능도 없다. 그래서 보통 프로토타입을 기반으로 상속을 흉내내도록 구현해 사용한다.

    참고로 최근의 ECMA6 표준에서는 Class 문법이 추가되었다. 하지만 문법이 추가되었다는 것이지, 자바스크립트가 클래스 기반으로 바뀌었다는 것은 아니다.

- 클래스를 통해 객체를 보다 간결하게 작성할 수 있으며, 생성자 기능은 hood 아래에서 발생하는 작업을 보다 정확하게 설명합니다.

## 생성자 함수

- 함수 초기화 시 메소드를 추가하는 것 (다른 언어의 클래스와 유사)

```js
function Vehicle(make, model, color) {  
  this.make = make,  
  this.model = model,  
  this.color = color,  
  this.getName = function () {  
    return this.make + " " + this.model
  }
}
```



## Prototype

```javascript
function Person() {}
Person.prototype.eyes = 2;
Person.prototype.nose = 1;
var kim  = new Person();
var park = new Person():
console.log(kim.eyes); // => 2  
// 이게 가능한 이유는 kim의 proto가 Person()의 prototype object를 가리키기 때문
...

//Person.prototype이라는 빈 Object가 어딘가에 존재하고, Person 함수로부터 생성된 객체(kim, park)들은 어딘가에 존재하는 Object에 들어있는 값을 모두 갖다쓸 수 있습니다.
```

- 자바스크립트에는 **Prototype Link** 와 **Prototype Object**라는 것이 존재합니다. 그리고 이 둘을 통틀어 **Prototype**이라고 부릅니다. 프로토타입을 좀 안다는 것은 이 둘을 완벽히 이해하고 갖고 놀 수준이 되었다는 뜻입니다.

### Prototype Object

- 함수가 정의될 때, 아래의 일이 동시에 이루어진다.

  1.해당 함수에 Constructor(생성자) 자격 부여 : `var a = new obj()` 를 사용할 수 있는 자격

  2.해당 함수의 Prototype Object 생성 및 연결 

  - 해당 함수는 prototype 속성을 통해 prototype object에 접근 가능
  - Prototype Object는 일반적인 객체와 같으며 기본적인 속성으로 `constructor`와 `__proto__`를 가지고 있습니다.
    - `__proto__`는 Prototype Link이다.

### Prototype Link

- Prototype Object가 지닌 기본 속성 중 하나인 `__proto__`
- `__proto__`모든 객체가 빠짐없이 가지고 있는 속성
- `__proto__`는 객체가 생성될 때 조상이었던 함수의 Prototype Object를 가리킵니다.
- 만약 특정 속성을 불렀는데 해당 속성이 현재 함수에 없으면 찾을 때까지 상위 프로토타입을 탐색한다. 최상위 Object의 prototype object까지 도달했는데 못찾을 경우 undefined 리턴한다. 이렇게 연결되어있는 형태를 Prototype chain이라고 한다.

## Class

- JavaScript의 클래스는 실제로 추가 기능을 제공하지 않으며, 더 깨끗하고 간결한 문법을 제공한다는 점에서 프로토타입과 상속에 대해 "syntactical sugar"를 제공하는 것으로 설명되기도 합니다. 

- `JavaScript 클래스는 함수 유형`입니다. 클래스는 `class` 키워드로 선언됩니다. 함수 식 구문을 사용하여 함수를 초기화하고 클래스 식 구문을 사용하여 클래스를 초기화합니다.

```js
// Initializing a constructor function
function Hero(name, level) {
    this.name = name;
    this.level = level;
}


// Initializing a class definition
class Hero {
    constructor(name, level) {
        this.name = name;
        this.level = level;
    }
}

//초기화 구문의 유일한 차이점은 함수 대신 class 키워드를 사용하고 constroctor() 메서드 내에 속성을 할당하는 것입니다.
```

### Defining Methods

생성자 기능은 일반적으로 초기화 대신 프로토타입에 직접 메소드를 할당하는 것입니다.

- constructor.js

  ```js
  function Hero(name, level) {
      this.name = name;
      this.level = level;
  }
  
  // Adding a method to the constructor
  Hero.prototype.greet = function() {
      return `${this.name} says hello.`;
  }
  ```

- class.js

  ```js
  // 클래스를 사용하면 문법, 구문이 단순화되고 메서드를 클래스에 직접 추가할 수 있습니다. ES6에서 소개한 방법을 사용하면 선언을 정의하는 것이 훨씬 더 코드들이 간결해지게 됩니다.
  class Hero {
      constructor(name, level) {
          this.name = name;
          this.level = level;
      }
  
      // Adding a method to the constructor
      greet() {
          return `${this.name} says hello.`;
      }
  }
  ```

### Extending a Class

- 생성자 기능 및 클래스의 이점은 상위 항목을 기반으로 새 객체로 확장할 수 있다는 것입니다. 이렇게 하면 비슷하지만 몇 가지 추가 기능 또는 그 이상의 특정 기능이 필요한 개체에 대한 코드가 반복되지 않습니다.

  새로운 생성자 function이 생성이 되면 `call()` 메서드를 부모에게 연결해줍니다. 예를들어, `Mage`라는 class를 만들고 `call()`을 사용하여 `Hero` 의 속성을 할당하고, 추가 속성을 추가할 것입니다.

- constructor.js

  ```js
  // Creating a new constructor from the parent
  function Mage(name, level, spell) {
      // Chain constructor with call
      Hero.call(this, name, level);
  
      this.spell = spell;
  }
  
  // 포인트는 Hero는 Mage에 새로운 인스턴스가 생성이 되면서 프로퍼티를 할당받게 됩니다.
  const hero2 = new Mage('Lejon', 2, 'Magic Missile');
  ```

  

- class.js

  ```js
  // Creating a new class from the parent
  class Mage extends Hero {
      constructor(name, level, spell) {
          // Chain constructor with super
          super(name, level);
          //ES6 클래스 중 super라는 키워드가 있습니다. 이 키워드는 call대신 부모 function에 대한 기능을 액세스하게 됩니다. 부모의 class를 확장하기 위해 사용됩니다.
          
          // Add a new property
          this.spell = spell;
      }
  }
  //동일한 방식으로 Mage에 새로운 인스턴스를 생성합니다.
  const hero2 = new Mage('Lejon', 2, 'Magic Missile');
  ```

## 팩토리 함수

- 팩토리 함수는 객체(아마도 새로운)를 반환하는 클래스나 생성자가 아닌 함수입니다.
  JavaScript에서는 모든 함수가 객체를 반환 할 수 있습니다. new 키워드가 없으면 팩토리 함수입니다.
- 클래스의 복잡성과 새 키워드로 뛰어들어 가지 않고도 객체 인스턴스를 쉽게 생성 할 수 있다.

```js
const Animal = function(name){
    const animal = {};
    animal.name = name;
    animal.walk = function(){
        console.log(this.name + " walks");
    }
    return animal;
};
// 함수를 더 추가하기 위해 믹스인을 사용합니다. 믹스인은 자체적으로 상태가 존재하지 않지만, 아래의 코드에서 객체에 메서드를 추가하는 방법은 좋은 방법 입니다.
const canKill = {
  kill() {
    console.log("I can kill")
  }
}

k1 = Object.assign(Animal("k1"), canKill)
```

하지만 cankill을 여러번 사용해야할 때, 아래와 같이 팩토리를 만들 수 있다.

```js
const KillingAnimal = function(name) {
  const animal = Animal(name)
  const killingAnimal = Object.assign(animal, canKill)
  return killingAnimal;
}

k2 = KillingAnimal("k2")
monty = KillingAnimal("monty")
k2.kill()
k2.walk()
```



### **simple factory**

- 다른 객체의 생성을 캡슐화하는 객체입니다." ES6에서 그것은 "new"에 의해 instathiated되는 생성자가 될 수 있습니다.

```js
//Definition of class
class User { 
	constructor(typeOfUser){
        this._canEditEverything = false; 
        if (typeOfUser === "administrator") {               
            this._canEditEverything = true;
        }
    } 
	get canEditEverything() { return this._canEditEverything; }
	
}//Instatiation
let u1 = new User("normalGuy");
let u2 = new User("administrator");
```

### Factory Method 

- 하나의 메소드를 정의합니다. 예를 들어 *createThing* 은 반환 할 항목을 결정하는 하위 클래스에 의해 재정의됩니다. Factories과 Products은 클라이언트가 그것들을 사용하기 위한 인터페이스를 따릅니다.

새로운 메소드를 만드는 대신 "new"를 계속 사용하면서 ES6에서 클래스를 확장하여 구현할 수 있습니다.

```js
//Class
class User {
    constructor(){
        this._canEditEverything = false;
    } 
    get canEditEverything() { return this._canEditEverything; }
}//Sub-class
class Administrator extends User {
    constructor() {
        super();
        this._canEditEverything = true;
    }
}//Instatiation
let u2 = new Administrator();
u2.canEditEverything; //true
```

###  Abstract Factory

- 구체적인 클래스를 지정하지 않고 관련 오브젝트 또는 종속 오브젝트의 *families* 을 생성하기위한 인터페이스를 제공합니다.

여기서 중요한 점은 "*families*"이라는 단어입니다. 이 예제를 계속 사용하면 ES6에서 "인터페이스"(클래스) 및 "구체적인 클래스"(하위 클래스)로 구현할 수 있습니다. TypeScript에서 (c #에서와 같이) _interface_exists는 정의없이 (구현없이) 클래스가 구현합니다. 그러나 JS (ES6 포함)에서는 그렇지 않습니다. 그래서 우리는 그것을 확장하는 클래스와 하위 클래스를 사용합니다. 그래서 우리는 서브 클래스 "Administrator", "Editor", "Publisher"등을 가질 수 있습니다.



## 요약

- 클래스의 `constructor`는 동작하기 위해 `new` 키워드가 필요하다. constructor는 우리가 아래처럼 호출할 경우에만 동작하는 것을 의미한다.
- 클래스 메소드는 열거가 불가능하다.(non-enumerable) Javascript에서 객체의 각 속성에는 `enumerable` 플래그가 존재한다. 이 플래그는 해당 속성에서 수행할 일부 작업에 대한 가용성을 정의한다. 클래스에서는 `prototype`에 정의된 모든 메소드에 대해 이 플래그를 `false`로 설정한다.
- 클래스에 `constructor`를 추가하지 않으면, 다음과 같이 기본으로 빈 `constructor`가 자동적으로 추가된다.
- 클래스 안의 코드는 항상 strict 모드이다. 이것은 오류가 없는 코드를 작성하거나, 잘못된 입력 또는 코드 작성 중에 작성된 구문 오류, 다른 곳에서 참조 된 코드를 실수로 제거하는 것을 방지해 준다.
- 클래스 선언은 `호이스팅` 되지 않는다. Javascript에서 호이스팅은 모든 선언이 현재 스코프 위에 자동으로 이동하는 메커니즘이다. 호이스팅은 변수 또는 함수가 선언되기 전에 사용할 수 있게 한다.
- 클래스는 생성자 함수나 객체 리터럴과 같은 속성에 접근해서 값을 할당하는 것을 허용하지 않는다. 오직 함수나 getter / setter로만 가능하다. 따라서 클래스엔 `property:value` 직접 할당 값이 없다.