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
date: 2020-03-05 12:00:00
lastmod: 2020-03-05 12:00:00
sitemap :
  changefreq : daily
  priority : 1.0
---

타입의 placeholder, Associated type에 대해서 알아보자.

<br>

## Associated Types

**Associated type**은 **type**에 대한 **placeholder 이름**을 주며, 프로토콜에서 사용됩니다. (placeholder는 임시로 자리를 차지하는 Character 또는 String 값) **Associated type**에 실제로 사용될 타입은 ptorocol이 채택될 때까지 명시되지 않습니다. Associated type은 코드상에서 `associatedtype`이라는 키워드로 표기됩니다.

> placeholder

In computer programming, a placeholder is a character, word, or string of characters that may be used to take up space until such a time that the space is needed. For example, a programmer may know that she needs a certain number of values or variables, but doesn't yet know what to input

<br>

`Item`이라는 associated type을 선언한 프로토콜 `Conatiner`를 예시로 보겠습니다.

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

`Contianer` 프로토콜에 있는 `associatedtype `로 선언된 `Item`은 **어떻게 저장되어야하는지** 또는 **어떠한 타입**으로 사용되어야하는지 명시하지 않았습니다. 프로토콜은 3가지 기능을 명시했고, `Container`로 간주되기 위해서 `Item`으로 사용될 **어떤 타입(any type)**이 제공해야 합니다. **채택 타입(conforming type)**은 이 3가지 요구사항이 충족되는 한, 추가적인 기능을 제공할 수 있습니다.

`Container` 프로토콜을 채택한 **어떤 타입(any type)**은 저장하는 값의 타입을 지정할 수 있어야합니다. 특히 적절한 타입의 item만이 container에 추가될 수 있고, subscript에 의해 리턴되는 item의 타입을 명확하게 해야합니다. 

이러한 요구사항들을 정의하기 위해서, `Container` 프로토콜은 어떤 container에 사용될 타입이 무엇인지 알지 못하는 상태에서 **container가 가지게 될 요소의 타입**을 참조하는 방법이 필요합니다. `Container` 프로토콜은 `append(_:)` 메소드에 전달되는 **어떤 값(any value)**은 container의 요소 타입과 동일한 타입을 가져야한다는 것, 그리고 container의 subscript에 의해 리턴되는 값 또한 container의 요소 타입과 동일한 타입을 가져야한다는 것을 명시해야합니다.

이를 위해서 `Container` 프로토콜은 `Item`이라는 **associated type**을 정의하며, 코드상에서는 `associatedtype Item` 와 같이 표기됩니다. 프로토콜은 직접 `Item`이 무엇인지 정의하지 않으며 그 정보는 나중에 **어떤 채택 타입(any conforming type)**으로 제공하기 위해 남겨둡니다. `Item` 별칭(alias)은 `Container`의 item의 타입을 참조할 방법, 그리고 `append(_:)` 메소드와 `subscript`에 사용될 타입을 정의하는 방법을 제공하며, `Container`의 예상 동작이 시행되도록 한다.

<br>

아래 코드는 generic을 사용하지 않고 구현된 `IntStack`입니다.

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

`IntStack`은 `Container` 구현에 사용할 적절한 `Item`을 `Int` 타입이라고 명시했습니다.

```swift
typealias Item = Int
```

위 정의는 `Container` 프로토콜 정의에 있어 `Item`의 **추상 타입(the abstract type)**을 `Int`라는 **하나의 구체적인 타입(a concrete type)**으로 바꿉니다.

사실 **Swift의 타입 추론** 덕분에, 구체적으로 `Item`이 `Int`타입이라고 `IntStack`의 정의 부분에 선언할 필요는 없습니다. `IntStack`이 `Container` 프로토콜의 필수기능들을 전부를 채택하기 때문에, Swift는 `append(_:)` 메소드의 파라미터와 subscript의 리턴타입을 간단히 보는 것만으로도 사용하기 적절한 `Item`을 추론할 수 있습니다. 실제로, `typealias Item = Int` 코드라인을 지우게 되면, 모든게 정상적으로 작동하는데, 그 이유는 어떤 타입이 Item으로 사용될지 명확하기 때문입니다.

<br>

> Summary

- Protocol에서 명시되는 associated type은 타입을 명시하지 않습니다.
- Associated type의 타입은 protocol을 채택한 클래스에서 선언됩니다.
- Associated type 구현 방법은 두가지가 있습니다.
- `typealias`를 사용한 방법과 protocol에 사용된 추상 타입(the abstract type)을 특정 타입으로 모두 구현하는 방법입니다.

위 언급한 associated type 구현 중 후자 방법은 구현 중 실수의 우려가 있습니다.
{: .notice--warning}

<br>

### Extending an Existing Type to Specify an Associated Type

 [Adding Protocol Conformance with an Extension](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html#ID277)에 명시된 것 처럼, 채택(conformance)을 프로토콜에 추가하기 위해서 기존에 존재하는 타입을 **확장**시킬 수 있습니다. 이는 Associated type을 가지고 있는 protocol을 포함합니다.

Swift의 `Array` 타입은 이미 `append(_:)` 메소드, `count` 프로퍼티, 그리고 `Int` index로 요소를 가져오는 `subscript`를 제공합니다. 이 3가지 기능은 `Container`의 필수요건들과 일치합니다. 이는 `Array`가 프로토콜을 채택한다고 선언함으로써, `Container` 프로토콜에 맞게 `Array`를 확장할 수 있습니다. [Declaring Protocol Adoption with an Extension](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html#ID278)에 설명 된대로 빈 extension으로 작업을 수행합니다.

```swift
extension Array: Container {}
```

`Array`의 `append(_:)` 메소드와 subscript는 Swift가 `Item`에 사용할 적절한 타입을 추론할 수 있게 해줍니다. 이 extension을 정의한 후, 어떤 `Array`든지 `Container`처럼 사용할 수 있습니다.

<br>

### Adding Constraints to an Associated Type

프로토콜의 associated type에는 **'타입 제약(type constraints)'**을 추가할 수 있습니다. Associated type에 들어오게 될 타입은 명시된 제약 조건을 충족해야합니다. 예를 들어, 아래 코드는 item은 container에서 `Equatable`을 채택한 타입만 사용되게 `Container`에서 정의되어있습니다. Equatable을 채택하지 않은 타입을 associated type에 지정하게 되면 컴파일 에러가 발생합니다.

```swift
protocol Container {
    associatedtype Item: Equatable
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

이 `Container` 버전을 채택하려면, container의 `Item` 타입은 `Equatable` 프로토콜을 채택해야힙니다.

<br>

#### The Original Document

- [Associated Types in Swift document](https://docs.swift.org/swift-book/LanguageGuide/Generics.html#ID189)