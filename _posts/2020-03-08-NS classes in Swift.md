---
title: "[iOS] NS Classes in Swift"
header:
  teaser: /assets/images/ios-posts2.jpeg
  overlay_image: /assets/images/ios-posts2.jpeg
  overlay_filter: 0.2
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - iOS
tags:
  - NS
  - NS Classes
date: 2020-03-08 09:00:00
lastmod: 2020-03-08 12:00:00
sitemap :
  changefreq : daily
  priority : 1.0
---

Swift에서 사용되는 "NS" 출신 Classes는 어떤 원리로 사용되고 있는지 알아보자.

### 들어가기 전에

Observer pattern 중 Notification 메커니즘을 공부하다 `Notification`과 `NSNotification`의 차이가 궁금해서 알아보게 되었습니다. 그래서 이번 포스트에서는 `Notification`와 `NSNotification`을 예제로 사용할 것입니다.

Cocoa와 Cocoa Touch의 간단한 설명을 참고하시면 내용을 이해하는데 도움이 됩니다.

- [Cocoa, Cocoa Touch](https://corykim0829.github.io/ios/Cocoa-CocoaTouch/)

<br>

`NSNotification`과 `Notification`의 연관 관계를 찾던 중, Apple developer 문서에서는 다음과 같은 문서가 있었습니다.

### Classes Bridged to Swift Standard Library Value Types

Use bridged reference types when you need reference semantics or Foundation-specific behavior.
Reference semantics 또는 Foundation 특정 행동이 필요하면 연결된 참조 타입을 사용하시오.

이 부분에 대해서 명확한 이해가 가지 않았는데, 아래 `iOS 13 Programming Fundamentals with Swift: Swift, Xcode, and Cocoa Basics` 책의 **Some Foundation Classes** 일부분입니다.

글을 보면 두 객체간 연결다리를 짓는 행위인 **"Bridging"**에 대해서 조금 더 이해가 될 것입니다.

<br>

## Foundation Classes

Cocoa의 Foundation 클래스는 기본 데이터 타입과 Cocoa와 의사소통의 기반을 형성해줄 유틸리티를 제공합니다. 더 많은 정보는 Apple의 Foundation framework documentation 페이지에서 Foundation 클래스 리스트를 참고하면 좋습니다.

많은 상황에서, 암묵적으로 Foundation 클래스를  Swift 클래스로 방식으로 사용할 수 있습니다. 그 이유는 Swift의 클래스와 Foundation 클래스 사이에 **연결다리**를 잇는 **Swift의 기능** 때문입니다. `String`은 `NSString`에, `Arrray`는 `NSArray`에 **연결다리**가 형성되어있어, `String`과 `NSString`은 서로 **캐스팅**될 수 있으며, `Array`와 `NSArray`도 마찬가지로 서로 캐스팅 될 수 있습니다. 하지만 사실 우리는 캐스팅을 거의 할 필요가 없습니다. 그 이유는 Objective-C API는 우리가 `NSString` 또는 `NSArray`를 넘겨주기를 원할 때마다, 이 타입들은 Swift 번역(translation)을 통해 API는 `String` 또는 `Array`로 변경되기 때문입니다. 그리고 `String`과 `Array`를 Foundation에서 사용하면, `NSString`과 `NSArray`의 내장된 많은 프로퍼티와 메소드를 사용하는 것과 동일합니다.

Swift Foundation **"overlay"**는 네이티브 Swift 인터페이스를 다른 많은 Foundation 타입 앞으로 가져옵니다. Swift 인터페이스는 Foundation 클래스를 뜻하는 `NS` 접두사를 제거함으로써 구별되는데; Objective-C의 `NSData`는 Swift의 `Data`를 통해 접근 가능하며 Objective-C의 `NSDate`도 마찬가지로 Swift `Date`를 통해 접근 가능합니다. (정말 원한다면, 여전히 `NSData`와 `NSDate`를 직접 사용할 수 있습니다.) Swift와 Objective-C 타입은 서로 연결다리가 형성되어있으며, API는 **Swift 타입을 우선적으로 표시**하므로 캐스팅 및 전달이 우리가 예상한대로 작동될 것입니다. Swift 타입은 많은 Objective-C에는 없는 많은 편리함을 가지고 있습니다. Swift 타입들은 `Equatable`, `Hashable`, `Comparable`와 같은 적절한 Swift 프로토콜의 채택을 선언할 수 있고, 그리고 몇몇의 경우에 Objective-C 타입은 레퍼런스 타입(classes)인 반면 structs인 값 타입으로 사용될 수 있습니다.

두가지 종류의 브리징이 있다. `String`과 `Array`는 네이티브 Swift 타입으로, 독립적인 존재입니다. 반면에 `Date`와 `Data`는 Swift 네이티브 타입이 아니라, `NSDate`와 `NSData`의 **껍데기(facades)**라고 할 수 있습니다. 이는 Cocoa의 Foundation 프레임워크가 없다면 `Date`와 `Data`를 사용할 수 없습니다.

---

<br>

**Briding**과 **overlay**에 대해 이해가 됐다면 이제 실제 사용되는 클래스인 `Notification`과 `NSNotification`의 차이를 살펴보도록 하겠습니다.

Swift에서 사용되는 Notification은 다음과 같이 정의되어있습니다.

### Notification

A container for information broadcast through a notification center to all registered observers.
Notification center를 통해 등록된 모든 observer에게 브로드캐스트되는 정보를 담는 그릇이다.

#### Declaration

```swift
struct Notification
```

#### Using Reference Types

```swift
typealias Notification.ReferenceType
```

An alias for this value type's equivalent reference type.
값 타입의 참조 타입에 대한 별칭입니다.

<br>

### NSNotification

An **object** containing information broadcast to registered observers **that bridges to [`Notification`](https://developer.apple.com/documentation/foundation/notification)**; use `NSNotification` when you need **reference semantics** or **other Foundation-specific behavior**.
등록된 observer에게 브로드캐스트 하는 정보를 담은 객체로 `Notification`으로 연결되어있습니다. reference semantics 또는 Foundation 특정 동작이 필요할 때 NSNotification을 사용하십시오.

<br>

`NSNotification`의 Objective-C 문서에는 다음과 같이 정의가 되어있습니다.

```
A container for information broadcast through a notification center to all registered observers.
```

Swift의 `Notification`과 동일합니다.

즉, 위에서 언급한 것과 같이 `Notification`과 `NSNotification`은 연결다리가 있기 때문에 서로 캐스팅될 수 있다는 뜻입니다. 하지만 언제든지 원한다면 `NSNotification`을 직접적으로 사용할 수도 있습니다.

**Important** <br>
Swift의 Foundation 프레임워크로 **overlay**는 `Notification` 구조체를 제공하며, 이 구조체는 `NSNotification` class로 `bridging` 되어있습니다.
{: .notice--warning}

---

<br>

공식문서에는 이렇게 간단하게 정의가 되어있지만, Xcode에서 구체적으로 어떻게 구현되어있는지 살펴볼 수 있다. `Notification` struct는 다음과 같은 구조를 가지고 있습니다.

```swift
public struct Notification : ReferenceConvertible, Equatable, Hashable {

    public typealias ReferenceType = NSNotification
    public typealias Name = NSNotification.Name
    public var name: Notification.Name

    public var object: Any?
    public var userInfo: [AnyHashable : Any]?
    public init(name: Notification.Name, object: Any? = nil, userInfo: [AnyHashable : Any]? = nil)
  
    public func hash(into hasher: inout Hasher)
    public var description: String { get }
    public var debugDescription: String { get }
    public static func == (lhs: Notification, rhs: Notification) -> Bool
    public var hashValue: Int { get }
}
```

중요하게 봐야할 것은 상단에 선언된 2개의 `typealias`와 `name` 프로퍼티입니다. 먼저 `Name`을 보면 `typealias`를 사용하여 `NSNotification.Name`이라는 타입의 별칭으로 `Notification` 구조체 내에서 `Name`이라는 별칭을 사용하겠다고 선언하였고, 구조체에서 해당 타입 값을 저장할 수 있도록 `name` 프로퍼티를 따로 선언하였다.

마찬가지로 `typealias`를 사용하여 `ReferenceType`이라는 별칭을 `NSNotification` 타입을 대신해 사용하겠다고 선언하였다. 하지만 여기서 사용되는 `ReferenceType`은 [Associated Type](https://corykim0829.github.io/swift/Associated-Type/)이기 때문에 `ReferenceConvertible` 프로토콜에서 `ReferenceType`의 타입을 결정한다.

<br>

## ReferenceConvertible

A decoration applied to types that are backed by a Foundation reference type.
Foundation 참조 타입이 지원하는 타입에 적용되는 decoration이다.

여기서 사용되는 `ReferenceCovertible` protocol은 [Associated Type](https://corykim0829.github.io/swift/Associated-Type/)을 사용하는 프로토콜이다.

```swift
public protocol ReferenceConvertible : CustomDebugStringConvertible, CustomStringConvertible, Hashable, _ObjectiveCBridgeable {
    associatedtype ReferenceType : NSObject, NSCopying
}
```

NS가 붙은 클래스들을 Swift의 구조체로 연결다리를 형성하기 위해서는 구체적으로 `ReferenceConvertible`이라는 프로토콜을 사용하여 구현한다. 이 프로토콜은 다른 NS classes를 기반으로 만들어진 Swift structures에서도 확인할 수 있다.

<br>

#### Reference

- iOS 13 Programming Fundamentals with Swift: Swift, Xcode, and Cocoa Basics
- [NSNotification](https://developer.apple.com/documentation/foundation/nsnotification)
- [Notification](https://developer.apple.com/documentation/foundation/notification)
- [ReferenceConvertible](https://developer.apple.com/documentation/foundation/referenceconvertible)