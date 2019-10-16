---
title: "Swift 톺아보기 #2 - Closure"
header:
  teaser: /assets/images/swift-logo.jpg
  overlay_image: /assets/images/swift-logo-horizon.png
  overlay_filter: 0.1
categories:
  - Swift
tags:
  - Data Structure
  - Algorithm
---



함수 중심 프로그래밍의 핵심 클로저, 그리고 이를 활용한 고차함수까지 파헤쳐보자. struct와 class의 차이로 value, reference에 대해 공부해보자



1. Closure
2. 고차 함수 high-order function
3. Struct
4. 객체 중심 프로그래밍
5. 흐름 제어



### 1. Closure

모든 함수는 클로저, 클로저는 함수이거나 아닐수도 있다.

Swift는 함수 중심 프로그래밍(Functional Programming)이기 때문에 **함수**를 타입으로 지정하거나 인자값으로 넘기거나 리턴값으로 사용할 수 있다.

이러한 특징 때문에 클로저를 사용하면 상당히 편하다.

```swift
// 함수
func plus(a: Int, b: Int) -> Int { return a + b}

// 클로저
let plusClosure1: (Int, Int) -> Int = { (a, b) in return a + b }
let plusClosure2 = { (a: Int, b: Int) -> Int in return a + b }
```



클로저에서 외부 변수 값을 사용할 때에는 값을 가지고 있는게 아니라 refernce, 참조한다.

reference는 capture와 반대개념이다.

```swift
var intValue = 10
let increment = { (n:Int) in
    intValue = intValue + n
}
increment(5)
print(intValue)   //15

let c10 = increment
intValue = 100
c10(10)
print(intValue)   //110
```



클로저는 함수라고 생각하면 편하다. reference가 다른 기존의 함수들과 동일하게 작용한다.



#### 클로저 선언과 캡처목록

`캡처리스트`라고 한다.

```swift
var intValue = 10
let increment = {
    [intValue] (n:Int) in
    print(intValue + n)
}
intValue = 100
increment(5) //15
print(intValue) //여전히 100
```

캡처리스트 사용을 권장한다!



### 2. 고차 함수 high-order function

클로저 다양한 표현식!

클로저 안에서 **타입추론**을 할 수 있어서 타입을 생략할 수 있다.



#### filter reduce

map은 for문을 대신해 사용하는거라면

filter for문에서 if문을 같이 쓰는 패턴을 추상화했다.

reduce는 안에 있는 element들을 element들끼리 연산해서 결과물을 만든다. 배열보다 한차원 작은게 값이 된다.

1차원 배열 -> 값, 2차원 배열 -> 1차원 배열



#### 클로저 다양한 표현식

```swift
let numberArray = [2, 8, 1, 3, 5]
let resultArray = numberArray.map(squared) // 결과 값은 [4, 64, 1, 9, 25]
let result1 = numberArray.map({ (n : Int) -> Int in return n*n }) let result2 = numberArray.map({ (n : Int) -> Int in n*n })
let result3 = numberArray.map({ n in return n*n })
let result4 = numberArray.map({ n in n*n })
let result5 = numberArray.map({ $0 * $0 }) let result6 = numberArray.map() { $0 * $0 } let result7 = numberArray.map { $0 * $0 }
```



array에 flatMap, compactMap을 사용하면 optional값들이 제거된다.

요즘에는 compact 권장



```swift
func complex(from array: [Int]) -> Int {
    return array.filter({ $0 % 2 == 0 || $0 % 3 == 0})
                .map({ $0 * 5})
                .reduce(0, { $0 + $1 }) // 처음에는 $0에 0이 들어간다. $1에는 첫 엘리먼트 값. reduce 결과값이 항상 $0에 들어간다.!!
}
```



fizzbuzz ~



### 3. Struct

swift는 struct를 매우 권장한다. 둘다 잘 쓰기 어렵다.

private 으로 프로퍼티 못한다.



다른 언어에서 OOP에서 class로 이야기 하는 것 -> struct



기본적으로 internal이 생략되어있다.

같은 프로젝트면 같은 모듈이다.

private이 요즘은 fileprivate으로 동작된다.



#### struct vs class

struct로 만든 모든 변수들은 대부분은 stack에 생겼다가 사라진다. 메모리가 string처럼 복잡한 애들은 heap에 생기기도 한다. 대부분은 stack에서 사용을 하고 return되면 사라진다.

struct는 값이 복사된다. 새롭게 값이 생긴다.

class에서는 값이 reference로 값이 복사된다. indirect로 **포인터**만 생기는거다!



swift에서는 struct가 더 가볍고 빠르기 때문에 권장한다.

struct는 scope 벗어나면 값이 소멸된다. optional이면 nil되면 소멸



### 4. 객체 중심 프로그래밍 OOP

EBS 다큐멘터리 동과 서 (철학 다큐멘터리 10년 전꺼)

Objective - 객관적인



동양은 context로 분류한다. 서양은 분류를 어떻게 잘하는가 중요하다. 분류하는 기준을 객관적으로 잡는 것을 중요하게 생각한다. 속성이 무엇인지 등등



#### 동과 서 본론

초기화된 상태가 객체를 사용할 수 있다.

초기화 함수가 반드시 있어야한다.



클래스 코드 : 일반화, 추상화

객체 인스턴스 : 구체화, 개별화



### 클래스와 객체 인스턴스

클래스도 메모리 어딘가 올라가는 부품처럼

모든 언어들이 클래스를 메모리에 로딩한다. 그 클래스를 지칭해주는 포인터가 있다.

메모리 구조를 인스턴스로 만들면?! Heap에 인스턴스가 생기고 이를 myPen이라는 포인터가 가리킨다.

var my pen = NSPen()



### 앱을 만드려면...

스위프트 표준 라이브러리(아무것도 import안해도 쓸 수 있는거) - struct로

코코아 프레임워크 (SDK) - 대부분 클래스로 되어있다.



### 객체 중심 프로그래밍

- 프로퍼티 property와 메소드 method
- 캡슐화 encapsulation
- 상속 inheritance
- 다형성 polymorphism
- 클래스 객체 class와 객체 인스턴스 instance
- 객체 디자인 패턴 design pattern



### Identity vs equality

몇개의 포인터가 이 클래스를 참조하고 있는지 reference counting을 해야하기 때문에 class는 조금 더 복잡한 일이 생긴다.

그래서 같은 클래스를 가리키고 있다면 identity가 같은 것 (===)

다른 클래스를 가리키지만 공교롭게 프로퍼티들이 모두 같은 값을 가지고 있다면 equality (==)



### Value vs Reference

Call by value, Call by reference



실제로 struct는 copy on right?? 때문에 달라지는 순간 capture로 바뀐다.



reference때문에 부작용이 생길 수 있다.



class의 루트 클래스는 NSObject 안써도 된다.



### 다형성

다른 언어와 동일, 리스코프 치환에 맞게, override한게 실행되는 것



### 생성자 상속과 재정의

지정생성자가 필요하다.

Designated Initializer!



스위프트에서는 자기 것을 먼저 초기화 하고 super를 부를 수 있다.

이것이 safety check!



### 확장

#### 서브스크립트

대괄호 접근 가능



#### 프로토콜 : 지켜야할 규칙

추상화된 메소드들의 집합, 자바로치면 interface

protocol 메소드라 생각하면 편하다. getter, setter가 있어서



protocol 다중 상속 가능, composition도 된다.



클래스를 꼭 상속받지 않아도, ~~

그 방법은 익스텐션!