---
layout: post
title: "JS 33 concepts - 30_상속, 다형성, 코드의 재사용성"
author: "Oizys18"
categories: [Language]
comments: true
tags: [Javascript,JS33Concepts,Language]
---


## 클래스 상속

![https://miro.medium.com/max/1520/1*UZKbYsRuM1OVW3__aZdPdw.png](https://camo.githubusercontent.com/e50828727a6ca3779a7c45cff14196027304f2fc/68747470733a2f2f6d69726f2e6d656469756d2e636f6d2f6d61782f313532302f312a555a4b62597352754d314f5657335f5f615a645064772e706e67)

- JS는 프로토타입과 프로토타입 체인 개념을 사용해 상속한다. 

### Non JS Classical Inheritance

- non-JS의 클래스 상속은 보통 클래스를 확장 할 때 부모 클래스와 자식 클래스가 별도의 엔티티 인 동작을 부모 클래스에서 자식으로 복사한다. 
- 클래스에서 인스턴스 또는 객체를 만들 때 두 가지 동작의 또 다른 복사본이 발생하며 둘 다 별도의 `Entity`다. 
- 자식 클래스가 상속된 이후엔 사본이기 때문에 연결되어 있지 않고, 따라서 둘은 별도의 개체다. 
- 속성 및 동작이 아래로 흐른다.

### JS  Prototypal Inheritance (Behavior delegation pattern)

- 상속된 객체는 `__proto__`로 연결되어 있다.(`new`키워드로 생성됨!)

- JavaScript에서 객체를 만들 때 속성이나 동작을 복사하지 않고 링크를 만듭니다. 클래스가 확장되는 경우에도 비슷한 종류의 연결이 생성됩니다.  

- 모든 화살표는 행동 위임 링크(프로토타입 체인)이기 때문에 전통적인 비 JS 상속과 비교하여 반대 방향으로 이동합니다. 

- 

   

## ES5

```js
//SuperType constructor function
function SuperType(firstName, lastName){
	this.firstName = firstName,
	this.lastName = lastName,
	this.friends = ["Ashwin", "Jadeja"]
}

//SuperType prototype
SuperType.prototype.getSuperName = function(){
	return this.firstName + " " + this.lastName;
}

//SubType prototype function
// call을 사용하여 SuperType생성자 함수를 호출
// Call은 SubType생성자 함수를 사용하여 생성되는 
// 객체의 컨텍스트에서 SuperType생성자 함수를 실행
// SuperType의 인스턴스 속성을 상속 한 후, SubType생성자 함수에 age속성을 추가
function SubType(firstName, lastName, age){
	//Inherit instance properties
	SuperType.call(this, firstName, lastName);
	this.age = age;
}

//Inherit methods and shared properties
SubType.prototype = new SuperType();

//Add new property to SubType prototype
SubType.prototype.getSubAge = function(){
	return this.age;
}

//Create SubType objects
var subTypeObj1= new SubType("Virat", "Kohli", 26);
var subTypeObj2 = new SubType("Sachin", "Tendulkar", 39);

//Modify the friends property using the subTypeObj1
subTypeObj1.friends.push("Amit");

console.log(subTypeObj1.friends);//["Ahswin", "Jadega", "Amit"]
console.log(subTypeObj2.friends);//["Ashwin", "Jadega"]

//subTypeObj1
console.log(subTypeObj1.firstName); //Output: Virat
console.log(subTypeObj1.age); //Output: 26
console.log(subTypeObj1.getSuperName()); //Output: Virat Kohli
console.log(subTypeObj1.getSubAge()); //Output: 26

//subTypeObj2
console.log(subTypeObj2.firstName); //Output: Sachin
console.log(subTypeObj2.age); //Output: 39
console.log(subTypeObj2.getSuperName()); //Output: Sachin Tendulkar
console.log(subTypeObj2.getSubAge()); //Output: 39
```

![img](https://camo.githubusercontent.com/ce21f1d4f13243dbdcdc054c6c22162e3ca85fd7/68747470733a2f2f6d69726f2e6d656469756d2e636f6d2f6d61782f313739342f312a6a475a483142545a5462716872447a31664f733247512e706e67)

![img](https://camo.githubusercontent.com/a730c243609115d2406c0d369a6566b83e61b6b4/68747470733a2f2f6d69726f2e6d656469756d2e636f6d2f6d61782f313539362f312a4b41467130754c612d476c5464543232653559574c772e706e67)

## [ES6](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)

```js
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
```

- `class` 키워드를 통해 클래스 선언 가능!

- `class` 문법 안에 있는 코드는 항상 strict mode 로 실행

- ES6 클래스는 초기화 함수가 호출 되도록 보장함으로써 코드를 보다 안전 하게하며,
  해당 데이터에서 작동하고 유효한 상태를 유지하는 고정 함수 세트를 보다 쉽게 정의 할 수 있도록 합니다.

- **함수 선언**과 **클래스 선언**의 중요한 차이점은 함수 선언의 경우 [호이스팅](https://developer.mozilla.org/ko/docs/Glossary/Hoisting)이 일어나지만, 클래스 선언은 그렇지 않다는 것입니다. 클래스를 사용하기 위해서는 클래스를 먼저 선언 해야 하며, 그렇지 않으면, 다음 아래의 코드는 [ReferenceError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError) 에러를 던질 것입니다.

  - ```js
    const p = new Rectangle(); // ReferenceError
    
    class Rectangle {}
    ```

- 정적 메소드나 프로토타입 메소드가 `this` 값 없이 호출될 때, `this` 값은 메소드 안에서 `undefined`가 됩니다.

  - ```js
    class Animal { 
      speak() {
        return this;
      }
      static eat() {
        return this;
      }
    }
    
    let obj = new Animal();
    obj.speak(); // Animal {}
    let speak = obj.speak;
    speak(); // undefined
    
    Animal.eat() // class Animal
    let eat = Animal.eat;
    eat(); // undefined
    ```

- 전통적 방식의 함수기반의 구문으로 작성된 경우, 메서드 호출에서의 오토박싱은 *this* 초기값 기준의 non-strict mode로 발생하게 됩니다.  만일 초기값이 `undefined`,이면, *this*는 전역객체로 설정됩니다.

  - ```js
    function Animal() { }
    
    Animal.prototype.speak = function() {
      return this;
    }
    
    Animal.eat = function() {
      return this;
    }
    
    let obj = new Animal();
    let speak = obj.speak;
    speak(); // global object
    
    let eat = Animal.eat;
    eat(); // global object
    ```

### `static`키워드 

- 클래스를 위한 정적(static) 메소드를 정의
- 정적 메소드는 클래스의 인스턴스화([instantiating](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Introduction_to_Object-Oriented_JavaScript#The_object_(class_instance))) 없이 호출되며, 클래스의 인스턴스에서는 호출할 수 없습니다. 정적 메소드는 어플리케이션(application)을 위한 유틸리티(utility) 함수를 생성하는데 주로 사용됩니다.
- 즉, 그냥 인스턴스에게 넘겨주지 않고 부모 클래스만 가지고 있을 메소드를 생성할 때 사용한다.

```js
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    static distance(a, b) {
        const dx = a.x - b.x;
        const dy = a.y - b.y;

        return Math.hypot(dx, dy);
    }
}

const p1 = new Point(5, 5);
const p2 = new Point(10, 10);
p1.distance;  //undefined
p2.distance;  //undefined

console.log(Point.distance(p1, p2)); // 7.0710678118654755
```

### `extends` 키워드

- *클래스 선언*이나 *클래스 표현식*에서 다른 클래스의 **자식 클래스**를 생성하기 위해 사용됩니다.
- subclass에 constructor가 있다면, "this"를 사용하기 전에 가장 먼저 super()를 호출해야 합니다.

```js
class Animal { 
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    console.log(this.name + ' makes a noise.');
  }
}

class Dog extends Animal {
  speak() {
    console.log(this.name + ' barks.');
  }
}
```

### class 표현식

```js
// unnamed
let Rectangle = class {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
console.log(Rectangle.name);
//output : "Rectangle"


// named
let Rectangle = class Rectangle2 {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
console.log(Rectangle.name);
//output: "Rectangle2"
```

- **class 표현식**은 class를 정의 하는 다른 방법입니다. Class 표현식은 이름을 가질 수도 있고, 갖지 않을 수도 있습니다. 이름을 가진 class 표현식의 이름은 클래스의 body에 대해 local scope에 한해 유효합니다.

### [Object.setPrototypeOf()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/setPrototypeOf)

- https://infoscis.github.io/2018/01/25/ecmascript-6-expanded-object-functionality/
- 클래스는 생성자가 없는 객체(non-constructible)을 확장할 수 없습니다. 만약 기존의 생성자가 없는 객체을 확장하고 싶다면, 이 메소드를 사용하세요. [`Object.setPrototypeOf()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/setPrototypeOf):

```js
let person = {
    getGreeting() {
        return "Hello";
    }
};
let dog = {
    getGreeting() {
        return "Woof";
    }
};
// prototype은 person
let friend = Object.create(person);
console.log(friend.getGreeting());                      // "Hello"
console.log(Object.getPrototypeOf(friend) === person);  // true
// prototype을 dog로 설정
Object.setPrototypeOf(friend, dog);
console.log(friend.getGreeting());                      // "Woof"
console.log(Object.getPrototypeOf(friend) === dog);     // true
```

```js
var Animal = {
  speak() {
    console.log(this.name + ' makes a noise.');
  }
};

class Dog {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(this.name + ' barks.');
  }
}
var d = new Dog('Mitzie');
d.speak(); // Mitzie barks.

Object.setPrototypeOf(d, Animal);
d.speak(); // Mitzie makes a noise.

```

