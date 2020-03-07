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

타입없는 타입, Associated type에 대해서 알아보자

<br>

## Associated Types

**Associated type**은 **type**에 대한 **placeholder 이름**을 주며, 프로토콜에서 사용된다. (placeholder는 임시로 자리를 차지하는 Character 또는 String 값이라고 할 수 있다.) **Associated type**에 실제로 사용될 타입은 ptorocol이 채택될 때까지 명시되지 않는다. Associated type은 코드상에서 `associatedtype`이라는 키워드로 표기된다.

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

이 프로토콜은 container에 있는 item이 **어떻게 저장되어야하는지** 또는 **어떠한 타입**으로 사용되어야하는지 명시하지 않았다. 프로토콜은 오직 `Container`로 간주되기 위해서 **어떤 타입(any type)**이 제공해야 하는 3가지 기능의 부분을 명시했다. **채택 타입(conforming type)**은 이 3가지 요구사항이 충족되는 한, 추가적인 기능을 제공할 수 있다.

`Container` 프로토콜을 채택한 **어떤 타입(any type)**은 저장하는 값의 타입을 지정할 수 있어야한다. 특히 적절한 타입의 item만이 container에 추가될 수 있고, subscript에 의해 리턴되는 item의 타입을 명확하게 해야한다. 

이러한 요구사항들을 정의하기 위해서, `Container` 프로토콜에 특정 container에 사용될 타입이 무엇인지 알지 못하는 상태에서 **container가 가지게 될 요소의 타입**을 참조하는 방법이 필요하다. `Container` 프로토콜은 `append(_:)` 메소드에 전달되는 **어떤 값(any value)**은 container의 요소 타입과 동일한 타입을 가져야한다는 것, 그리고 container의 subscript에 의해 리턴되는 값 또한 container의 요소 타입과 동일한 타입을 가져야한다는 것을 명시해야한다.

이를 위해서 `Container` 프로토콜은 Item이라는 **associated type**을 정의하며, 코드상에서는 `associatedtype Item` 와 같이 표기된다. 프로토콜은 직접 `Item`이 무엇인지 정의하지 않으며 그 정보는 나중에 **어떤 채택 타입(any conforming type)**으로 제공하기 위해 남겨둔다. `Item` alias는 `Container`의 item의 타입을 참조할 방법, 그리고 append(_:) 메소드와 subscript에 사용될 타입을 정의하는 방법을 제공하며, `Container`의 예상 동작이 시행되도록 한다.

<br>

아래 코드는 generic을 사용하지 않고 구현된 `IntStack`이다. 

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

`IntStack`은 `Container` 구현에 사용할 적절한 `Item`을 `Int` 타입이라고 명시했다.

```swift
typealias Item = Int
```

위 정의는 `Container` 프로토콜 정의에 있어 `Item`의 **추상 타입(the abstract type)**을 `Int`라는 **하나의 구체적인 타입(a concrete type)**으로 바꾼다.

Swift의 타입 추론 덕분에, `Int` 타입의 하나의 구체적인 `Item`을 꼭 `IntStack` 정의로 선언할 필요는 없다. `IntStack`이 `Container` 프로토콜의 필수요건들 전부를 채택하기 때문에, Swift는 `append(_:)` 메소드의 파라미터와 subscript의 리턴타입을 간단히 보는 것만으로도 사용하기 적절한 `Item`을 추론할 수 있다. 실제로, `typealias Item = Int` 코드라인을 지우게 되면, 모든게 정상적으로 작동하는데, 그 이유는 어떤 타입이 Item으로 사용될지 명확하기 때문이다.

<br>

> Summary

- Protocol에서 만들어진 associated type은 타입을 명시하지 않는다.
- Associated type의 타입은 protocol을 채택한 클래스에서 지정된다.
- Associated type 구현 방법은 두가지가 있다.
- `typealias`를 사용한 방법과 protocol에 사용된 추상 타입(the abstract type)을 특정 타입으로 모두 구현하기

위 언급한 associated type 구현 중 후자 방법은 구현 중 실수의 우려가 있다.
{: .notice--warning}

<br>

### Extending an Existing Type to Specify an Associated Type

 [Adding Protocol Conformance with an Extension](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html#ID277)에 명시된 것 처럼, 채택(conformance)를 프로토콜에 추가하기 위해서 기존에 존재하는 타입을 **확장**시킬 수 있다. 이는 Associated type이 있는 protocol을 포함한다.

Swift의 `Array` 타입은 이미 `append(_:)` 메소드, `count` 프로퍼티, 그리고 `Int` index로 요소를 가져오는 subscript를 제공한다. 이 3가지 기능은 `Container`의 필수요건들과 일치한다. 이는 `Array`가 프로토콜을 채택한다고 선언함으로써, `Container` 프로토콜에 맞게 `Array`를 확장할 수 있다. [Declaring Protocol Adoption with an Extension](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html#ID278)에 설명 된대로 빈 extension으로 작업을 수행한다.

```swift
extension Array: Container {}
```

`Array`의 `append(_:)` 메소드와 subscript는 Swift가 `Item`에 사용할 적절한 타입을 추론할 수 있게 해준다. 이 extension을 정의한 후, 어떤 `Array`든지 `Container`처럼 사용할 수 있다.

<br>

### Adding Constraints to an Associated Type

프로토콜의 associated type에 '타입 제한'을 추가하여 적합한 타입이 해당 제한 조건을 충족하도록 요구할 수 있다. 예를 들어, 아래 코드는 item은 container에서 equatable하게 `Container`에서 정의되어있다.

```swift
protocol Container {
    associatedtype Item: Equatable
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

이 `Container` 버전을 채택하려면, container의 `Item` 타입은 `Equatable` 프로토콜을 채택해야한다.

