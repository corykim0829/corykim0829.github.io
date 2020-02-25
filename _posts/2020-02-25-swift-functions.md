---
title: "[Swift] Global & Nested Functions"
header:
  teaser: /assets/images/swift-logo.jpg
  overlay_image: /assets/images/swift-logo-horizon.png
  overlay_filter: 0.1
categories:
  - Swift
tags:
  - function
  - global function
  - nested function
---

스위프트의 함수 종류 - Global, nested function

<br>

## Functions In Swift

함수는 특정 일을 수행하는 독립적인 코드 덩어리다. 우리는 함수에 이름을 주어 이 함수가 어떤 일을 하는지 구분할 수 있으며, 이 이름은 우리가 일을 처리할 필요가 있을 때, 함수를 '호출'하는데 사용된다.

```swift
func shout() {
    print("HEY!")
}

shout()
```

스위프트에서 사용되는 함수 문법 구조는 C 스타일 함수나 Objective-C 스타일 메소드를 표현할 수 있다. 특히 **파라미터** 부분에서 유연성이 돋보인다. 파라미터는 default 값을 제공할 수 있어 함수를 간편하게 호출할 수 있다. 파라미터가 'in-out 파라미터'로 전달되면, 함수의 실행이 완료하면 전달된 파라미터는 함수 내부에서 수정된 내용이 반영된다.

스위프트의 모든 함수는 **타입**을 가지고 있으며, **함수의 파라미터 타입**과 **반환 타입**으로 구성되어있다. 우리는 이 타입을 스위프트의 다른 타입들처럼 사용할 수 있다.  이는 다른 함수로 파라미터로 함수를 전달하는 것을 쉽게 해준다. 함수는 중첩된 함수 scope 안에 유용한 기능을 캡슐화하기 위해 다른 함수 안에 작성될 수 있다.



## Nested Functions

우리가 이제까지 스위프트에서 봐왔던 함수들은 대부분 global functions(전역함수)이다. global function은 global scope에서 정의된 함수이다.

함수는 다른 함수 내부에서도 정의될 수 있는데, 이를 nested functions, 중첩 함수라고 한다.

Nested function은 기본적으로 바깥으로부터 숨겨져 있지만, enclosing function(감싸고 있는 함수)에 의해 호출된다. Enclosing function는 가지고 있는 nested function 중 하나를 리턴하여 nested function이 **다른 scope**에서도 쓰일 수 있도록 할 수 있다.

아래 예제는 nested function을 리턴하는 코드이다.

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backward ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```

