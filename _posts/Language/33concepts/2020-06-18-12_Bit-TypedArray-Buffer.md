---
layout: post
title: "JS 33 concepts - 12_비트 연산자, 형식화 배열, 버퍼(배열)"
author: "Oizys18"
categories: [Language]
comments: true
tags: [Javascript,JS33Concepts,Language,]
---
* TOC
{:toc}
* * *
- 컴퓨터는 모든 데이터를 이진 형식 (0,1)로 저장한다. 그 후 UTF-8과 같은 인코딩을 사용하여 저장된 비트조합을 문자,숫자, 혹은 다른 기호로 맵핑한다.
- 비트 수준에서 변수와 상호 작용하는 방법입니다. 비트는 일반적으로 부동 소수점 및 정수로 변환되므로 정보를 쉽게 소화 할 수 있습니다. 우리가 속도와 효율성을 중요시한다면 비트를 직접 처리하고 그 변환을 floats / int로 건너 뛰는 것이 유용 할 것입니다. 비트는 자바 스크립트의 변수보다 빠르지 만 번역 수준을 건너 뛰는 것보다 복잡합니다.

## &(AND)

-  `&&` 논리 연산자와 마찬가지로, 이 연산자는 비교된 비트가 모두 1과 0이면 다른 모든 경우에 1을 반환합니다. 양쪽에서 숫자를 선택한 다음(이진 형식이 아니라 숫자) 비트를 하나씩 비교합니다.
- 

## | (OR)

- `||`과 매우 유사합니다.
- 두 이진수를 비교할 때 각 자리를 확인하고 둘 다 0이면 0 반환, 하나라도 1이면 1 반환

## ~ (NOT)

- 모든 비트를 반대로 한다. (양수 -> 음수, 음수 -> 양수)
- 단, `~양수`를 하게되면 결과가 `-(양수+1)`이 된다. 음수를 표시하려면 숫자의 비트를 바꾼다음 1을 추가해야한다.

## ^ (XOR)

-  XOR 연산자 또는 독점적인 OR로 알려져 있습니다. `&`, `|` 연산자와 동일하게 비교를 하는 방식을 차별화하면서 양쪽의 숫자를 취합니다.
- `^` 연산자는 `1`과 `0`을 비교하는 특정 경우에만 `1`을 반환합니다.
-  각 비트를 비교하고 1이 1개만 있을 때만 1을 반환한다. 
   - `1 ^ 0 -> 1` , `1 ^ 1 -> 0`  
## Shifting 연산자

### 1. `<<` 왼쪽 시프트

-  2를 곱하는 것과 같은 효과
- `<<` 연산자는 숫자의 모든 비트를 `n`번 바꿉니다. 여기서 유의해야 할 것은 번호를 옮길 때 발생하는 빈 공간은 모두 `0`으로 채워져 있다는 점입니다.

### 2. `>>` 부호 유지 오른쪽 시프트

- 2로 나누는 것과 같은 효과 (나머지 버림!)

- 반면에 `>>` 연산자는 오른쪽으로 이동합니다. 이 변환 연산자와 이전 변환 연산자의 차이점은 이 연산자가 양의 숫자의 비트를 0초로 채우고 음의 숫자의 비트를 1로 채우는 것입니다.
  
  - ```
    base 2 : 1010 (-8 + 0 + 2 + 0 = -6)
    // >> 이동은 양의 숫자 비트를 0으로 채우고 음의 숫자 비트를 1로 채운다
    1010 >> 1 = 1101 (-8 + 4 + 1 = -3)    
    ```
    
  - 보통 숫자의 첫 번째 부분은 표지를 나타내기 위해 사용됩니다. 1이면 음수이고, 0이면 양수입니다. 따라서 `>>`의 이면에 있는 추론은 우리가 움직이고 있는 숫자의 표식을 지키기 위한 것입니다.

### 3. 부호버림 오른쪽 시프트 Zero-fill right shift: `>>>`

- 앞서 살펴 본 `>>`는 양의 숫자 비트를 0으로 채우고 음의 숫자 비트를 1로 채운다. 즉 음수는 음수, 양수는 양수를 유지한다. 

- 하지만 `>>>`은 빈공간을 0으로 채우는 시프트다. 

  ```
  base 2 : 1010 (-8 + 0 + 2 + 0 = -6)
  // >>> 이동은 빈공간을 0으로 채운다. 
  1010 >>> 1 = 0101 (4 + 1 = 5)    
  
  // 양수 비트 쉬프트에선 >>와 결과에 아무 차이 없다. 
  ```

## Binary 음수 계산

- 가장 왼쪽(가장 큰)  비트/자리 값이 음수를 나타내는 데 사용된다.
- 만약 양의 값 비트를 안다면 반전(`1 -> 0` `0 -> 1`) 후 1을 더하면 된다.

```
// 4 비트 작업 시 -7표시
base 2:   1001  = 1000 + 000 + 00 + 1                  
   aka:     -7  =  -8  +  0  +  0 + 1                 
// 양의 비트 값으로 음수 만들기
base2 : 7 = 0111 
값 반전    -> 1000
1을 더함   -> 1001 (base2 -7) 

// 4 비트 작업 시 -2 표시
base 2:   1110  = 1000 + 100 + 10 + 0
   aka:     -8  =  -8  +  4  +  2 + 0
```

## 비트마스크 (mask)

- masks는 달성하려는 항목에 따라 `1` 또는 `0`으로 설정할 비트만 있는 이진 형식의 숫자입니다. 또한 변경할 비트를 정의하는 플래그로도 사용된다고 말할 수 있습니다.  (`1010`,`1000` 등)

## JS 비트와이즈 연산자(Bitwise Operators) 활용

| 연산자                      | 용법    | 설명                                                         |
| --------------------------- | ------- | ------------------------------------------------------------ |
| Bitwise AND                 | a & b   | 왼쪽 피연산자와 오른쪽 피연산자의 비트가 모두 1 인 경우 각 비트 위치에 1을 반환한다. |
| Bitwise OR                  | a \| b  | 왼쪽 또는 오른쪽 피연산자의 비트가 하나 인 경우 각 비트에서 하나를 반환한다. |
| Bitwise XOR                 | a ^ b   | 한 비트의 비트가 왼쪽 피연산자와 오른쪽 피연산자 둘 다 아닌 경우 비트 위치의 1을 반환한다. |
| Bitwise NOT                 | ~ a     | 피연산자의 비트를 뒤집는다.                                  |
| 왼쪽 시프트 연산            | a << b  | a를 2 진수 표현 b 비트를 왼쪽으로 시프트하고 오른쪽에서 0을 시프트한다. |
| 부호있는 오른쪽 시프트 연산 | a >> b  | a를 2 진수로 b 비트를 오른쪽으로 시프트하고, 제거 된 비트를 제거한다. |
| 부호없는 우측 시프트 연산   | a >>> b | a를 2 진수로 b 비트를 오른쪽으로 시프트하고, 시프트 오프 한 비트를 버리고, 왼쪽에서 0을 시프트한다. |

![img](https://camo.githubusercontent.com/5215d2944a9bfc91d03fbbb8cd7d812e11efa3b7/68747470733a2f2f63646e2d696d616765732d312e6d656469756d2e636f6d2f6d61782f313630302f312a366a696d654964596a4e586e386f474b5730544d65772e706e67)


- 정수를 수동으로 추가하고 빼는 대신 비트와이즈 연산자는 각 정수를 나타내는 비트를 작업하여 직접 비교하고 조작할 수 있습니다. 따라서 0과 1에 따라 4비트 수(또는 3비트 또는 12비트)를 조작할 수 있으며, 각 비트는 ture/false 속성 중 하나를 나타냅니다.
```js
// Let's define an object that needs to be checked. In the
// real world, this might come from an API response, or user
// interactions, or a form, etc. You might not know it beforehand.
const myObject = {
  foo1: false,
  foo2: true,
  foo3: false,
  foo4: true
}
// Let's also set up some constants to make code easier to 
// read later on. These could obviously take many forms, or be set
// up in different ways, but I find this the most intuitive to read:
const HAS_FOO1 = 1;         // 0001
const HAS_FOO2 = 1 << 1;  // 0010
const HAS_FOO3 = 1 << 2;  // 0100
const HAS_FOO4 = 1 << 3;  // 1000
// Construct your bitwise number. How you do this will depend
// on your use-case, but here's one way to do it: Checking object
// keys manually and using if statements to add attributes one at
// a time.
let myBitNumber = 0;
if (myObject['foo1'] === true)
  myBitNumber = myBitNumber | HAS_FOO1;
  // This uses the bitwise | to form a union
if (myObject['foo2'] === true)
  myBitNumber = myBitNumber | HAS_FOO2;
if (myObject['foo3'] === true)
  myBitNumber = myBitNumber | HAS_FOO3;
if (myObject['foo4'] === true)
  myBitNumber = myBitNumber | HAS_FOO4;
console.log(myBitNumber.toString(2));
// 1010
/*
 * Our bitwise number is now "1010". That's because our second and
 * fourth attributes are true.
 * Think of it this way:
 *
 * | fourth | third | second | first | <= Attribute
 * |    1   |   0   |   1    |   0   | <= True/false
 *
 */
```

- 위와 같은 방법으로 bitwise 값을  여러 true / false 속성을 효율적으로 저장 및 비교할 수 있다.

### 다른 방법 (array 사용)

![https://cdn-images-1.medium.com/max/1600/1*QhPKjsxw22Hf5a3-cR96fQ.png](https://camo.githubusercontent.com/e181606cdd66fd636b948441e0dee228f030ad3c/68747470733a2f2f63646e2d696d616765732d312e6d656469756d2e636f6d2f6d61782f313630302f312a5168504b6a737877323248663561332d6352393666512e706e67)

![https://cdn-images-1.medium.com/max/1600/1*w6IRRQ8AxCLiQOLZYtNccA.png](https://camo.githubusercontent.com/1050c6a0bc1ad7de7b1090b71e325c034629f6c4/68747470733a2f2f63646e2d696d616765732d312e6d656469756d2e636f6d2f6d61782f313630302f312a773649525251384178434c69514f4c5a59744e6363412e706e67)

![https://cdn-images-1.medium.com/max/1600/1*cTfbvJIxJOGyvx2ysmi7Tg.png](https://camo.githubusercontent.com/7800372ebd799b427930ae56ce3de19c589cc865/68747470733a2f2f63646e2d696d616765732d312e6d656469756d2e636f6d2f6d61782f313630302f312a63546662764a49784a4f477976783279736d693754672e706e67)

