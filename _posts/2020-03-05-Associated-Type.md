---
title: "[Swift] Associated Type"
header:
  teaser: /assets/images/swift-logo.jpg
  overlay_image: /assets/images/swift-logo-horizon.png
  overlay_filter: 0.1
categories:
  - Swift
tags:
  - Associated Type
---

Associated Type에 대해서 알아보자

<br>

## Associated Types

**Associated type**은 type에 대한 placeholder 이름을 주며, 프로토콜에서 사용된다. (placeholder는 임시로 자리를 차지하는 Character 또는 String 값이라고 할 수 있다.) **Associated type**에 실제로 사용될 타입은 ptorocol이 채택될 때까지 명시되지 않는다. Associated type은 `associatedtype`이라는 키워드로 표기된다.

> placeholder

In computer programming, a placeholder is a character, word, or string of characters that may be used to take up space until such a time that the space is needed. For example, a programmer may know that she needs a certain number of values or variables, but doesn't yet know what to input

<br>

`Item`이라는 associated type을 선언한 프로토콜 `Conatiner`를 예시로 보자

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

이 프로토콜은 container에 있는 item이 어떻게 저장되어야하는지 또는 **어떠한 타입**으로 사용되어야하는지 명시하지 않았다. 프로토콜은 오직 Container로 간주되기 위해서 **어떤 타입(any type)**이 제공해야 하는 3가지 기능의 부분을 명시했다. **채택 타입(conforming type)**은 이 3가지 요구사항이 충족되는 한, 추가적인 기능을 제공할 수 있다.

`Container` 프로토콜을 채택한 **어떤 타입(any type)**은 저장하는 값의 타입을 지정할 수 있어야한다. 특히 적절한 타입의 item만이 container에 추가될 수 있으며, subscript의 리턴타입을 명확하게 해야한다. 

이러한 요구사항들을 정의하기 위해서, `Container` 프로토콜에 특정 container의 타입이 무엇인지 알지 못하는 상태에서 **container가 가지게 될 요소의 타입**을 참조하는 방법이 필요하다. `Container` 프로토콜은 `append(_:)` 메소드에 전달되는 **어떤 값(any value)**은 container의 요소 타입과 동일한 타입을 가져야한다는 것, 그리고 container의 subscript에 의해 리턴되는 값 또한 container의 요소 타입과 동일한 타입을 가져야한다는 것을 명시해야한다.

이를 위해서 `Container` 프로토콜은 Item이라는 **associated type**을 정의하며, `associatedtype Item` 으로 표기된다. 프로토콜은 `Item`이 무엇인지 정의하지 않으며 그 정보는 제공할 어떤 **채택 타입(conforming type)**으로 남겨둔다. `Item` alias는 `Container`의 item의 타입을 인용할 방법, 그리고 append(_:) 메소드와 subscript에 사용될 타입을 정의하는 방법을 제공하며, 예상했던 대로 처리되도록 `Container`가 ...

```swift
struct IntStack: Container {
    // original IntStack implementation
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
    // conformance to the Container protocol
    typealias Item = Int
    mutating func append(_ item: Int) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Int {
        return items[i]
    }
}
```

Swift의 타입 추론 덕분에, Int의 Item을 선언하지 않아도 된다. `IntStack`이 `Container` 프로토콜의 필수요건들 전부를 채택하기 때문에, Swift는 `append(_:)` 메소드의 파라미터와 subscript의 리턴타입을 간단히 보는 것만으로도 사용하기 적절한 Item을 추론할 수 있다. Indeed, typealias Item = Int 줄을 코드에서 지우게 되면, 모든게 정상저긍로 작동하는데, 그 이유는 어떤 타입이 Item으로 사용될지 명확하기 때문이다.



### Extending an Existing Type to Specify an Associated Type

우린 conformance를 프로토콜에 추가하기 위해서 존재하는 타입을 확장시킬 수 있다. 여기에 명시된 것 처럼. associated type이 있는 protocol을 포함한다.

Swift의 `Array` 타입은 이미 `append(_:)` 메소드, `count` 프로퍼티, 그리고 Int index로 요소를 가져오는 subscript를 제공한다. 이 3가지 기능은 Container의 필수요건들과 매치된다. 이는 `Array`가 프로토콜을 채택한다고 선언함으로써, `Container` 프로토콜에 맞게 `Array`를 확장할 수 있다. [Declaring Protocol Adoption with an Extension](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html#ID278)에 설명 된대로 빈 extension으로 작업을 수행한다.

```swift
extension Array: Container {}
```

`Array`의 `append(_:)` 메소드와 subscript는 Swift가 `Item`에 사용할 적절한 타입을 추론할 수 있게 해준다. 이 extension을 정의한 후, 어떤 `Array`든지 `Container`처럼 사용할 수 있다.

<br>

### Adding Constraints to an Associated Type

프로토콜의 associated type에 '타입 제한'을 추가하여 적합한 타입이 해당 제한 조건을 충족하도록 요구할 수 있다.

```swift
protocol Container {
    associatedtype Item: Equatable
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```