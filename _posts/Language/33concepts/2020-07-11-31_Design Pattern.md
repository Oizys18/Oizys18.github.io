---
layout: post
title: "JS 33 concepts - 31_설계패턴 Design Pattern"
author: "Oizys18"
categories: [Language]
comments: true
tags: [Javascript,JS33Concepts,Language]
---
* TOC
{:toc}
* * *
# 31_[설계패턴](https://en.wikipedia.org/wiki/Design_Patterns)

### 디자인 패턴에서 고려할 사항

1. Context: 어떤 상황에서 사용되는가?
2. Problem: 무엇을 해결하려 하는가?
3. Solution: 이 패턴을 사용해서 문제를 어떻게 해결하는가?
4. Implementation: 구현된 코드는 어떠한가?
   
### 창조 디자인 패턴

- **객체 인스턴스 생성을 위한 패턴**
  - 클라이언트와 그 클라이언트에서 생성해야 할 객체 인스턴스 사이의 연결을 끊어주는 패턴
- 객체 생성 메커니즘을 처리하기위한 것
- 창조 디자인 패턴은 기본적으로 객체의 생성 과정을 제어함으로써 문제를 해결한다.
- 종류
  - 생성자 패턴
  - 팩토리 패턴
  - 프로토타입 패턴
  - 싱글톤 패턴

### 구조 설계 패턴

- **클래스 및 객체들을 구성을 통해서 더 큰 구조로 만들 수 있게 해 주는 것과 관련된 패턴**
- 클래스 및 객체 구성과 관련이 있다. 
- 전체 시스템에 영향을주지 않고 하나 이상의 부품을 구조화하거나 재구성 할 수 있다. 
  - 즉, 기존 기능을 변경하지 않고도 새로운 기능을 얻을 수 있다.
- 종류
  - 어댑터 패턴
  - 복합 패턴
  - 데코레이터 패턴
  - 퍼사드 패턴
  - 플라이웨이트 패턴
  - 프록시 패턴

### 행동 디자인 패턴

- **클래스와 객체들이 상호작용하는 방법 및 역할을 분담하는 방법과 관련된 패턴**
- 객체 간의 통신을 개선하는 데 관련된다.
- 종류
  - 책임 사슬 패턴
  - 커맨드 패턴
  - 이터레이터 패턴
  - 중재자 패턴
  - 관찰자 패턴
  - 상태 패턴
  - 전략 패턴
  - 템플릿 패턴

# 창조 디자인 패턴

## Module Design Pattern

- **특정 컴포넌트의 코드를 다른 구성 요소와 독립적으로 유지하기 위해 사용한다.**
- **IIFE**를 통해 바로 비공개 변수를 만드는 디자인 패턴 
- [**Essential Reading**: Learn React from Scratch! (2019 Edition)](https://bit.ly/2TtV1sA)
- JS는 객체지향 언어와 비슷하게 사용하기 위해 `class`로 모듈을 사용한다. 
- 다른 클래스로 부터의 접근 및 상태를 보호하는 `encapsulation 캡슐화`를 사용한다. 
- 모듈패턴을 사용하면 public과 private 액세스 수준이 허용된다. 
- private 스코프를 허용하려면 IIFE를 사용한다. 

```js
var HTMLChanger = (function() {
  var contents = 'contents'

  var changeHTML = function() {
    var element = document.getElementById('attribute-to-change');
    element.innerHTML = contents;
  }

  return {
    callChangeHTML: function() {
      changeHTML();
      console.log(contents);
    }
  };

})();

HTMLChanger.callChangeHTML();       // Outputs: 'contents'
console.log(HTMLChanger.contents);  // undefined
```

- 반환하려고 하는 객체를 반환하기 전에 private한 변수나 함수를 인스턴스화한다.
- 클로저 외부 코드는 private 변수와 동일 스코프가 아니므로 해당 변수에 접근이 불가하다.

### Revealing Module Pattern

- 개인 범위에서 모든 함수와 변수를 간단하게 정의하고 공개하고자하는 개인 기능에 대한 포인터가 있는 익명 객체를 반환하는 업데이트 된 모듈패턴입니다.
- encapsulation(캡슐화)를 유지하고, 객체 리터럴로 반환되는 특정 변수와 메소드를 공개하는 것이 목적

```js
var Exposer = (function() {
  var privateVariable = 10;

  var privateMethod = function() {
    console.log('Inside a private method!');
    privateVariable++;
  }

  var methodToExpose = function() {
    console.log('This is a method I want to expose!');
  }

  var otherMethodIWantToExpose = function() {
    privateMethod();
  }
  // 이런 식으로 밖에서 부르려는 함수에 대한 포인터가 있는 익명 객체를 반환
  return {
      first: methodToExpose,
      second: otherMethodIWantToExpose
  };
})();

Exposer.first();        // Output: This is a method I want to expose!
Exposer.second();       // Output: Inside a private method!
Exposer.methodToExpose; // undefined
// 직접적으로 부르는 건 안되지만 익명객체로는 불린다!
```

- 코드가 깔끔해지지만, private한 메소드 참조는 할 수 없다는 단점이 있다.
- 단위 테스트와 관련된 문제가 발생할 수 있습니다. 
- 마찬가지로 public한 함수는 오버라이드 될 수 없습니다.

## Prototype Design Pattern

- **성능이 중요한 상황에서 객체를 생성하는데 사용한다.**
- 프로토타입 디자인 패턴은 [JavaScript prototypical inheritance](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)에 의존한다. 
- 생성한 객체는 원래 객체의 복사본(얕은 복사)다. (Link다!)
- 대표적인 사례는 광범위한 데이터베이스 작업을 수행하여 애플리케이션의 다른 부분에 사용되는 객체를 만드는 것입니다. 이 광범위한 데이터베이스 작업을 수행하는 대신 다른 프로세스에서 이 객체를 사용해야하는 경우 이전에 만든 객체를 복제하는 것이 좋습니다.

```js
var TeslaModelS = function() {
  this.numWheels    = 4;
  this.manufacturer = 'Tesla';
  this.make         = 'Model S';
}

TeslaModelS.prototype.go = function() {
  // Rotate wheels
}

TeslaModelS.prototype.stop = function() {
  // Apply brake pads
}

// 아래와 같이 동기적으로 함수를 확장할 수도 있다.
TeslaModelS.prototype = {
  go: function() {
    // Rotate wheels
  },
  stop: function() {
    // Apply brake pads
  }
}
```

### Revealing Prototype Pattern

- 프로토타입 패턴의 변형
- 객체 리터럴을 반환하기 때문에 public와 private한 멤버와 함께 캡슐화(encapsulation)를 제공합니다.
- 객체가 반환하기 때문에 프로토타입 객체는 prefix로 **function**을 붙일 것입니다. 위의 예제를 확장하여 현재 프로토타입에 노출할 항목을 선택하여 접근 수준을 유지할 수 있습니다.

```js
var TeslaModelS = function() {
  this.numWheels    = 4;
  this.manufacturer = 'Tesla';
  this.make         = 'Model S';
}

TeslaModelS.prototype = function() {

  var go = function() {
    // Rotate wheels
  };

  var stop = function() {
    // Apply brake pads
  };

  return {
    pressBrakePedal: stop,
    pressGasPedal: go
  }

}();
```

##  [Singleton Design Pattern](https://ideveloper2.tistory.com/160)

- **싱글톤 패턴**은 전체 시스템에서의 하나의 인스턴스만 존재하도록 보장하는 객체 생성패턴 
- 모듈 패턴을 활용한 패턴
- 객체 리터럴도 모두 싱글톤 패턴 
- 싱글톤은 특정 클래스의 인스턴스를 **오직 하나**만 유지하고, 동일한 클래스로 새로운 객체를 생성해도 처음 만들어진 객체를 얻게 됩니다.
- **싱글톤 패턴**은 객체지향 언어에서 여러개의 인스턴스 생성을 피하는 유용한 패턴입니다. 
- JS의 객체리터럴 싱글턴 패턴
  1. 객체 리터럴만으로는 비공개 상태나 함수를 정의 할 수 없습니다. 따라서 외부에서 접근을 할 수 없도록 비공개 멤버를 정의해야 한다고 하는데요, 여기서 당연히 필요한 개념은 클로저입니다.
  2. 즉, 싱글톤 객체를 생성하기 위해선 **객체 리터럴 + 클로저**의 조합이 필요

```js
var singleton = (function() {
  var instance;
  var a = 'singleton';
  function init() {
    return {
      a: a,
      b: function() {
        console.log(a);
      }
    };
  }
  return {
    getInstance: function() {
      if (!instance) {
        instance = init();
      }
      return instance;
    }
  }
})();
var singletone1 = singleton.getInstance();
var singletone2 = singleton.getInstance();
console.log(singletone1 === singletone2); // true;
```

## 생성자 패턴 (Constructor Pattern)

- 클래스 기반의 디자인 패턴
- 생성자는 해당 함수로 정의된 메서드 및 속성으로 새 객체를 인스턴스화하는 데 사용할 수 있는 특수 함수이다.
- 생성자 패턴은 주어진 종류의 새 객체를 생성하기 위해 JavaScript에서 가장 일반적으로 사용되는 패턴 중 하나이다.
- `new` 키워드로 생성자 메서드를 호출하여 객체를 인스턴스화한다.

```js
// traditional Function-based syntax
function Hero(name, specialAbility) {
  // setting property values
  this.name = name;
  this.specialAbility = specialAbility;

  // declaring a method on the object
  this.getDetails = function() {
    return this.name + ' can ' + this.specialAbility;
  };
}

// ES6 Class syntax
class Hero {
  constructor(name, specialAbility) {
    // setting property values
    this._name = name;
    this._specialAbility = specialAbility;

    // declaring a method on the object
    this.getDetails = function() {
      return `${this._name} can ${this._specialAbility}`;
    };
  }
}

// creating new instances of Hero
const IronMan = new Hero('Iron Man', 'fly');

console.log(IronMan.getDetails()); // Iron Man can fly
```

## 팩토리 패턴 (Factory Pattern)

-  또 다른 클래스 기반 생성 패턴
- 객체 인스턴스화의 책임을 하위 클래스에 위임하는 범용 인터페이스를 제공
- 서로 다르지만 비슷한 특성이 많은 개체 컬렉션을 관리하거나 조작해야 할 때 자주 사용

```js
class BallFactory {
  constructor() {
    this.createBall = function(type) {
      let ball;
      if (type === 'football' || type === 'soccer') ball = new Football();
      else if (type === 'basketball') ball = new Basketball();
      ball.roll = function() {
        return `The ${this._type} is rolling.`;
      };

      return ball;
    };
  }
}

class Football {
  constructor() {
    this._type = 'football';
    this.kick = function() {
      return 'You kicked the football.';
    };
  }
}

class Basketball {
  constructor() {
    this._type = 'basketball';
    this.bounce = function() {
      return 'You bounced the basketball.';
    };
  }
}

// creating objects
const factory = new BallFactory();

const myFootball = factory.createBall('football');
const myBasketball = factory.createBall('basketball');

console.log(myFootball.roll()); // The football is rolling.
console.log(myBasketball.roll()); // The basketball is rolling.
console.log(myFootball.kick()); // You kicked the football.
console.log(myBasketball.bounce()); // You bounced the basketball.
```



# 구조 설계 패턴

## 어댑터 패턴 (Adapter Pattern)

- 한 클래스의 인터페이스가 다른 클래스의 인터페이스로 변환되는 구조적 패턴
- 이 패턴을 사용하면 호환되지 않는 인터페이스 때문에 다른 방법으로는 불가능했던 클래스가 함께 작동하도록 한다.
- 이 패턴은 종종 기존의 다른 오래된 API가 여전히 작동 할 수 있도록, 새로운 리팩토링 된 API에 대한 wrapper를 작성하는 데 사용된다. 

```js
// old interface
class OldCalculator {
  constructor() {
    this.operations = function(term1, term2, operation) {
      switch (operation) {
        case 'add':
          return term1 + term2;
        case 'sub':
          return term1 - term2;
        default:
          return NaN;
      }
    };
  }
}

// new interface
class NewCalculator {
  constructor() {
    this.add = function(term1, term2) {
      return term1 + term2;
    };
    this.sub = function(term1, term2) {
      return term1 - term2;
    };
  }
}

// Adapter Class
class CalcAdapter {
  constructor() {
    const newCalc = new NewCalculator();

    this.operations = function(term1, term2, operation) {
      switch (operation) {
        case 'add':
          // using the new implementation under the hood
          return newCalc.add(term1, term2);
        case 'sub':
          return newCalc.sub(term1, term2);
        default:
          return NaN;
      }
    };
  }
}

// usage
const oldCalc = new OldCalculator();
console.log(oldCalc.operations(10, 5, 'add')); // 15

const newCalc = new NewCalculator();
console.log(newCalc.add(10, 5)); // 15

const adaptedCalc = new CalcAdapter();
console.log(adaptedCalc.operations(10, 5, 'add')); // 15;
```

## 복합 패턴 (Composite pattern)

- 트리와 같은 구조로 객체를 구성하여, 전체 부분의 계층 구조를 나타내는 구조 설계 패턴

- 이 패턴에서 트리와 같은 구조의 각 노드는 개별 객체이거나 구성된 객체 모음 일 수 있다. 어쨌든 각 노드는 균일하게 처리된다.

  ![img](https://camo.githubusercontent.com/1b9aa5f49a17d73fa6d1214539a2f71dedac8f3f/68747470733a2f2f6d69726f2e6d656469756d2e636f6d2f6d61782f3337332f312a79416b6559434b44615251744d524156324b783573412e706e67)

## 데코레이터 패턴 (Decorator Pattern)

- 기존 클래스에 동작 또는 기능을 동적으로 추가하는 것에 초점을 둔 구조 설계 패턴
- 클래스 상속의 또다른 대안
- JavaScript를 사용하면 객체에 메서드와 속성을 동적으로 추가할 수 있기 때문에, JavaScript에서 데코레이터 유형 동작은 매우 쉽게 구현할 수 있다. 가장 간단한 접근방식은 객체의 속성을 추가하는 것이지만, 재사용 할 수는 없을 것이다.

## 퍼사드 패턴 (Facade Pattern)

-  JavaScript 라이브러리에서 널리 사용되는 구조 설계 패턴
-  jQuery와 같은 JavaScript 라이브러리에서 종종 보인다.
- 구현은 광범위한 동작을 가진 메소드를 지원할 수 있지만 이러한 메소드의 "facade(정면)" 또는 제한된 추상화만 사용을 위해 공개됩니다.
- 하위 시스템이나 하위 클래스의 복잡성으로부터 벗어나 사용 편의성을 위해 통합되고 단순한 공공 지향 인터페이스를 제공하기 위해 사용된다.

## 플라이웨이트 패턴 (Flyweight Pattern)

- 세밀한 객체(fine-grained objects)를 통해효율적인 데이터 공유에 중점을 둔 구조 설계 패턴
- 효율성 및 메모리 절약 목적으로 사용
- 모든 종류의 캐싱 목적으로 사용할 수 있다.

## 프록시 패턴 (Proxy Pattern)

-  대상 객체를 또다른 객체가 접근할 때, 접근을 통제하는 대리자 역할을 하는 구조적 디자인 패턴
- 일반적으로 대상 개체가 제약을 받고 모든 책임을 효율적으로 처리하지 못할 수있는 상황에서 사용된다. 
-  이 경우 프록시는 일반적으로 클라이언트에 동일한 인터페이스를 제공하고 과도한 압력을 피하기 위해 대상 객체에 대한 제어된 접근을 지원하는 간접적인 수준을 추가한다.
- 네트워크 요청이 많은 애플리케이션으로 작업 할 때 불필요하거나 중복 된 네트워크 요청을 피하는 데 매우 유용하다.
- ES6의 [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)와 [Reflect](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

# 행동 디자인 패턴 

## Observer Design Pattern

- **애플리케이션의 한 부분이 변경되는 경우가 많으며, 한 부분이 변경시 다른 곳도 업데이트해야하는 경우 사용한다.**
- observer 패턴은 단지 객체가 수정되면 변경이 발생한 종속 객체로 **broadcasts**한다.
- ex) model-view-controller (MVC) 아키텍처 : 모델 변경시 뷰도 업데이트된다. 이점은 모델에서 뷰를 분리하여 의존성을 줄이는 것
- 용어
  - Subject: 관찰자 목록을 유지하고 관찰자 추가 또는 제거를 용이하게 합니다.
  - Observer: 대상의 상태 변경을 알릴 필요가 있는 객체에 대한 업데이트 인터페이스를 제공합니다.
  - ConcreteSubject: 상태 변화에 대한 관찰자에게 알림을 알리고, ConcreteObservers의 상태를 저장합니다.
  - ConcreteObserver: ConcreteSubject에 대한 참조를 저장하고 상태가 주제와 일치하도록 관찰자에 대한 업데이트 인터페이스를 구현합니다.
- 독립된 객체 또는 **subject**을 구별하는 것이 중요
  - 단점 : observer 수가 증가함에 따라 성능이 크게 저하된다.

```js
var Subject = function() {
  this.observers = [];

  return {
    subscribeObserver: function(observer) {
      this.observers.push(observer);
    },
    unsubscribeObserver: function(observer) {
      var index = this.observers.indexOf(observer);
      if(index > -1) {
        this.observers.splice(index, 1);
      }
    },
    notifyObserver: function(observer) {
      var index = this.observers.indexOf(observer);
      if(index > -1) {
        this.observers[index].notify(index);
      }
    },
    notifyAllObservers: function() {
      for(var i = 0; i < this.observers.length; i++){
        this.observers[i].notify(i);
      };
    }
  };
};

var Observer = function() {
  return {
    notify: function(index) {
      console.log("Observer " + index + " is notified!");
    }
  }
}

var subject = new Subject();

var observer1 = new Observer();
var observer2 = new Observer();
var observer3 = new Observer();
var observer4 = new Observer();

subject.subscribeObserver(observer1);
subject.subscribeObserver(observer2);
subject.subscribeObserver(observer3);
subject.subscribeObserver(observer4);

subject.notifyObserver(observer2); // Observer 2 is notified!

subject.notifyAllObservers();
// Observer 1 is notified!
// Observer 2 is notified!
// Observer 3 is notified!
// Observer 4 is notified!
```

### [Publish/Subscribe 패턴 (발행-구독 패턴)](https://rinae.dev/posts/why-every-beginner-front-end-developer-should-know-publish-subscribe-pattern-kr)

![Pattern Notification](https://jistol.github.io/assets/img/softwareengineering/observer-pubsub-pattern/1.png)

- 알림을 수신하려는 객체(subscribers)와 이벤트를 발생 시키는 객체(publisher) 사이에 있는 주제/이벤트 채널을 사용
- 보통 Observer 패턴과 Pub-sub을 함께 사용한다.
- [Observer 패턴과의 차이점](https://jistol.github.io/software%20engineering/2018/04/11/observer-pubsub-pattern/)
  - 중간에 `Message Broker` 또는 `Event Bus`가 존재하는지 여부
  - Pub-Sub 패턴이 더 결합도가 낮다.(Loose Coupling) 
  - Pub-Sub은 Observer와 Subject가 서로를 전혀 인지하지 않아도 상관없다.  
  - Pub-Sub패턴은 대부분 비동기(asynchronous) 방식
  - Observer패턴은 단일 도메인 하에서 구현되어야 하나 Pub-Sub패턴은 크로스 도메인 상황에서도 구현 가능



## 책임 사슬 패턴 (Chain of Responsibility Pattern)

-  느슨하게 연결된 객체 체인을 제공하는 행동 디자인 패턴
- 각 객체는 클라이언트 요청에 따라 작동하거나 처리하도록 선택할 수 있다.

## 커맨드 패턴 (Command Pattern)

-  동작 또는 동작을 객체로 캡슐화하는 것을 목표로하는 행동 디자인 패턴
- 작업을 요청하거나 실제 구현을 처리하거나 처리하는 객체에서 메소드를 호출하는 객체를 분리하여 시스템과 클래스를 느슨하게 연결할 수 있다.

## 이터레이터 패턴 (Iterator Pattern)

-  기본 표현을 노출시키지 않고 집계 객체 요소에 순차적으로 액세스하는 방법을 제공하는 행동 설계 패턴
- 이터레이터들은 우리가 끝에 도달할 때까지 `next()`를 호출하여 한 번에 하나씩 순서가 정해진 일련의 가치들을 밟아 나가는 특별한 종류의 행동을 한다.
- ES6에서 Iterator와 Generator의 도입으로 인해 이터레이터 패턴의 구현이 매우 간단해졌다.

## 중재자 패턴 (Mediator Pattern)

- 일련의 객체가 서로 상호 작용하는 방식을 캡슐화하는 행동 디자인 패턴
- 느슨한 결합을 촉진하여 객체가 서로 명시적으로 참조되지 않도록하여 객체 그룹에 대한 중앙 권한을 제공합니다.

## 상태 패턴 (State Pattern)

- 특정 작업에 대한 대체 알고리즘을 캡슐화 할 수 있는 행동 설계 패턴
- 알고리즘 제품군을 정의하고 클라이언트 간섭이나 지식없이 런타임에 상호 교환 할 수있는 방식으로 알고리즘을 캡슐화한다.

## 템플릿 패턴 (Template Pattern)

- 알고리즘의 골격 또는 작업 구현을 정의하지만 일부 단계를 서브 클래스로 연기하는 동작 기반 디자인 패턴
- 서브 클래스는 알고리즘의 외부 구조를 변경하지 않고 알고리즘의 특정 단계를 재정의 할 수 있다.

## 참고

- [JS 4가지 디자인패턴](https://scotch.io/bar-talk/4-javascript-design-patterns-you-should-know#undefined)
- [Observer vs Pub-sub](https://hackernoon.com/observer-vs-pub-sub-pattern-50d3b27f838c)
- [디자인패턴의 종류](https://hyeonstorage.tistory.com/100)
- [**디자인 패턴 : 재사용 가능한 객체 지향 소프트웨어의 요소**](https://en.wikipedia.org/wiki/Design_Patterns) by *Erich Gamma, Richard Helm, Ralph Johnson and John Vlissides (Gang of Four)*
- [**자바 스크립트 디자인 패턴 알아보기**](https://addyosmani.com/resources/essentialjsdesignpatterns/book/) by *Addy Osmani*
- [**자바 스크립트 패턴**](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) by *Stoyan Stefanov*