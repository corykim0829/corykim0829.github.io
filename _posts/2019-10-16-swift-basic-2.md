---
title: "Swift 톺아보기 #2 - Closure, High-Order function, ..."
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
5. 생성자 상속과 재정의
6. Type Casting
7. 프로토콜
8. 익스텐션, 제네릭



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

요즘에는 compact를 권장한다.



```swift
func complex(from array: [Int]) -> Int {
    return array.filter({ $0 % 2 == 0 || $0 % 3 == 0})
                .map({ $0 * 5})
                .reduce(0, { $0 + $1 })
  // 처음에는 $0에 0이 들어간다. $1에는 첫 엘리먼트 값. reduce 결과값이 항상 $0에 들어간다.!!
}
```



클로저 응용은 더 깊이 정리해보는 걸로



### 3. Struct

swift는 struct를 매우 권장한다. 둘다 잘 쓰기 어렵다.

private 으로 프로퍼티를 선언 못한다.



다른 언어에서 OOP class로 이야기 하는 것이 swift에서는 struct라고 생각하면 된다.



#### 접근제어

public, open > internal > fileprivate > private

기본적으로 internal이 생략되어있다.

같은 프로젝트면 같은 모듈이다.

private이 요즘은 fileprivate으로 동작된다.



#### struct vs class

공통점

- 프로퍼티에 값을 저장할 수 있다.
- 함수로 원하는 기능을 제공할 수 있다.
- 서브스크립션으로 값에 접근할 수 있다.
- 초기 값을 위해 생성함수를 정의할 수 있다.

struct로 만든 모든 변수들은 대부분은 stack에 생겼다가 사라진다. 메모리가 string처럼 복잡한 애들은 heap에 생기기도 한다. 대부분은 stack에서 사용을 하고 return되면 사라진다.

**struct**는 값이 복사된다. 새롭게 값이 생긴다. struct는 상속 불가이며 `의미있는 값`이다. **Value semantic** (Direct)

**class**에서는 값이 reference로 값이 복사된다. indirect로 **포인터**만 생기는거다! 상속이 가능하며 상위/하위 클래스 타입으로 형변환이 가능. 소멸함수에서 불필요한 리소스를 해제한다. 인스턴스별 참조 개수 관리가 필요하며 `의미있는 레퍼런스`이다. **Reference semantic** (Indirect)



swift에서는 struct가 더 가볍고 빠르기 때문에 권장한다.

#### struct의 값 소멸 지점

struct는 scope 벗어나면 값이 소멸된다. optional이면 nil되면 소멸



### 4. 객체 중심 프로그래밍 OOP

#### 객체란 무엇인가?

OBJECT : 대상 = 사물, 서양 사람들은 자기를 기준으로 다른 물건들을 object라고 한다. 객관적인 기준으로 분류해서 다른 물건 즉 object를 구분한다.

`Objective - 객관적인` 이 단어는 서양에서 자주 쓰이는 단어이다. 객체 중심 프로그래밍이라는 것 자체가 동양인들에게는 낯설 수 있다. 



#### 언어와 생각하는 방식

##### 서양 사람 - 명사로 세상을 보는 사람

(Would you like to have) more tea?

##### 동양 사람 - 동사로 세상을 보는 사람

(차) 더 마실래?



동양은 context로 분류한다. 서양은 분류를 어떻게 잘하는가 중요하다. 분류하는 기준을 **객관적**으로 잡는 것을 중요하게 생각한다. 속성이 무엇인지 등등

초기화된 상태가 객체를 사용할 수 있다.

초기화 함수가 반드시 있어야한다.



클래스 코드 -> (구체화, 개별화) -> 객체 인스턴스

객체 인스턴스 -> (일반화, 추상화) -> 클래스 코드



### 클래스와 객체 인스턴스

클래스도 메모리 어딘가 올라가는 부품처럼 모든 언어들이 클래스를 메모리에 로딩한다. 그 클래스를 지칭해주는 포인터가 있다.

메모리 구조를 인스턴스로 만들면 Heap에 인스턴스가 생기고 이를 myPen이라는 포인터가 가리킨다.

var my pen = NSPen()



### 앱을 만드려면...

스위프트 표준 라이브러리(import 하지 않아도 쓸 수 있는거) - struct로 되어 있다.

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

CGPoint : 전형적인 struct type

```swift
let origin = CGPoint(x: 0, y: 0)
var other = origin
other.x += 10
```

엄밀히 얘기하면 실제로는 값을 바꾸는 시점에 복사가 일어난다. 그래서 그 전까지는 origin과 other 모두 동일한 객체를 가리키고 있다. 

struct는 copy on right?? 최적화?? 때문에



reference때문에 원하지않는 다른 객체의 값이 바뀌어 버리는 부작용이 생길 수 있다.

#### Value Types Reference

`inout`, `&`

```swift
let origin = CGPoint(x: 0, y: 0) var other = origin
other.x += 10
var another = origin
another.y += 5

func swapPoint(pointA : inout CGPoint, pointB : inout CGPoint) {
    let temp = pointA
     pointA = pointB
    pointB = temp
}

swapPoint(pointA: &other, pointB: &another)
print(other, another)
// (0.0, 5.0) (10.0, 0.0)
```

권장하지는 않는다.

사실 이것은 c, c++ API 호환성을 유지하기 위해서 들어간 것이다.



#### 구현 상속 inheritance

클래스 다중 상속은 지원하지 않는다.

class의 루트 클래스는 NSObject 안써도 된다.



### 다형성

다른 언어와 동일

```swift
 class Animal {
    func speak() {
			print("animal speak...")
    }
}

var animal = Animal()
animal.speak()

class Dog : Animal {
	 override func speak() {
			print("dog - bow-wow")
   }
}
class Cat : Animal {
    override func speak() {
			print("cat - meow")
    }
}

var dog = Dog()
dog.speak()

var cat = Cat()
cat.speak()

var animalArray : [Animal] = [animal, dog, cat]
for x in animalArray {
		x.speak()
}
```

마지막 부분에 `[Animal]` -> 리스코프 치환에 의해 각각의 타입에 맞게 그에 해당하는 override한게 실행되는 것 (다형성으로 되는 것)



### 5. 생성자 상속과 재정의

Designated initializer, Convenience initializer

1. 모든 클래스는 지정생성자, designated initializer 하나가 필요하다.
2. 더 편리한 convenience initializer는 designated Initializer를 호출해야한다.
3. subclass가 있을 때 designated initializer를 만들면 super class의 designated initializer를 호출해야한다.

iOS 개발하면서 그냥 따라 개발하면서 명확한 이유를 알지 못하였는데, 그 이유를 알게되었다!



```swift
class RecipeIngredient: Food {
     var quantity: Int
     init(name: String, quantity: Int) {
				self.quantity = quantity
				super.init(name: name)
     }
     override convenience init(name: String) {
		 		self.init(name: name, quantity: 1)
     }
}
```

보통 다른 언어들은 super class를 먼저 초기화하지만

{: .notice--note}
In Swift, 자기 것을 먼저 초기화 하고 super class initializer를 호출한다.
**이것이 safety check!**



위에 convenience initializer를 override한 이유는 상속한 Food class의 convenience initializer가 똑같이 String 하나를 인자로 받는게 존재하기 때문이다.



### 6. Type Casting

스위프트는 타입을 명시적으로 못 바꾼다. 새 객체를 만든다.

```objective-c
<Objective-C>
double value = 3.14
int convertedValue = (int)value
convertedValue = 1.234 //명시하지 않았지만 내부적으로 int 타입을 바꿔버림
```

```swift
<Swift>
var value: Double = 3.14
var convertedValue: Int = Int(value) convertedValue = 1.234 //에러!
```



#### is-a, as-a

`is-a` 관계 : To check the type of an Instance

`as-a` 관계 : To treat that Instance as a different superclass of subclass

as-a : type casting을 해보는것 물음표를 붙이면 실패하면 optional이 된다.



### 확장

#### 서브스크립트

array와 dictionary 접근하듯이 대괄호 접근 가능하게 만드는 확장 기능 같은 것



### 7. 프로토콜 : 지켜야할 규칙

추상화된 **구현해야하는** 메소드들의 집합, 자바로치면 interface

프로토콜 안에 함수와 프로퍼티를 선언할 수 있다.

프로토콜의 프로퍼티 또한 getter, setter가 있어서 protocol 메소드라 생각하면 편하다.



```swift
protocol Receivable {
    func received(data: Any, from: Sendable)
}

protocol Sendable {
    func send(data: Any)
}
```

```swift
protocol SomeProtocol {
    var settableProperty: String { get set }
    var readOnlyProperty: String { get }
    static var someTypeProperty: Int { get set }
    static var otherTypeProperty: Int { get }
}
```



```swift
public protocol CustomStringConvertible {
    public var description: String { get }
}

class WorkManager : CustomStringConvertible {
    var nativeArray = [10,12,24]
    var stringArray = ["Ebcd", "Bcd", "Acc", "Dedd"]
    
    var description : String {
        return "nativeArray = \(nativeArray)"
		}
}

var instance = WorkManager()
print(instance)
//nativeArray = [10, 12, 24]
```

`description` 은 getter이기 때문에 값을 **반환**하도록 코드를 작성해야한다.

어떤 struct나 class를 넘기면 print 함수는 description이 구현되어있는지 안되어있는지 확인하고 출력한다.

protocol은 다중 상속 가능, composition(protocol 여러개를 하나의 타입처럼 구현하는 것)도 된다.



OOP 에서 얘기하는 것은, NSObject에서 상속받아서 원하는 class들을 계층을 만들어서 세부적인 동작을 만들려면 특정한 class의 subclass를 만들어서 객체를 만드는게 OOP에서 하는 일 중에 하나

어떤 class를 만들고 싶다면 가장 근접한 class가 무엇이고 그것의 super class가 뭔지 선택에서 subclass를 만든다. view를 만들 때에도 마찬가지이다. 



#### Duck-type

요즘에 모든 현대 언어들, **Duck-type**이라 하는 것은 상속관계를 다 따라갈 필요가 없다. 

여러 클래스들을 꼭 상속받지 않아도, 특정 기능(cf 메소드)를 프로토콜을 상속처럼 특정 기능을 구현한다면 그 타입이라고 부를 수 있다는 것이다. 이러한 것을 Duck-type system이라고 한다.

Swift는 이러한 Duck-type을 권장하고 확장시키기를 원한다. 그 확장 방법은 익스텐션!



### 8. 익스텐션 & 제네릭

#### 익스텐션

원래있는 타입에다가 수평적으로 기능을 확장할 수 있다.

클래스, 구조체, 프로토콜, 제네릭 등 모든 타입

```swift
extension ExtensionTypeName {
		// 확장해서 구현할 내용
}

extension ExtensionTypeName : ProtocolName1, ProtocolName2 {
		// 프로토콜 구현 내용
}

extension Drink : CustomStringConvertible {
    var description: String {
       return "\(drinkName)"
    }
}

extension Drink : CustomDebugStringConvertible {
    var debugDescription: String {
				return "debug-\(drinkName)"
    }
}
```



view에서도 상속받아서 만들지 말고 protocol을 채택해서 확장해서 쓰는 것을 권장한다.

{: .notice--note}
**구현 상속보다는 인터페이스 상속!!**



#### 제네릭

복잡한 제네릭은 swift에서 잘 안될 때가 있다.

```swift
struct IntStack {
	 var items = [Int]()
	 mutating func push(_ item: Int) {
	 		items.append(item)
   }
	 mutating func pop() -> Int {
	 		return items.removeLast()
   }
}

struct Stack<Element> {
	 var items = [Element]()
	 mutating func push(_ item: Element) {
	 		items.append(item)
   }
	 mutating func pop() -> Element {
	 	  return items.removeLast()
   }
}

var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")
stackOfStrings.push("cuatro")
```

