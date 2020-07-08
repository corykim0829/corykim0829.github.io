---
title: "[iOS] Notification, an Instance of NSNotification"
header:
  teaser: /assets/images/ios-posts2.jpeg
  overlay_image: /assets/images/ios-posts2.jpeg
  overlay_filter: 0.2
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - iOS
tags:
  - Observer Pattern
  - Notification
  - NSNotification
date: 2020-03-03 12:00:00
lastmod: 2020-03-03 12:00:00
sitemap :
  changefreq : daily
  priority : 1.0
---

Observer Pattern의 Notification메커니즘에서 주고받는 notification object인 NSNotification에 대해서 알아보자.

### 들어가며

iOS의 observer pattern의 **Notification 메커니즘**을 알고 있어야 `NSNotification` 객체를 더 깊게 이해할 수 있습니다. 아래 포스트를 읽고 오시면 더욱 이해하기 쉽습니다.

- [**Observer Pattern, Notification**](https://corykim0829.github.io/ios/Observer-Pattern-Notification/)

<br>

## Notification

Observer pattern의 notification 메커니즘을 간단하게 설명해보면, 객체간 커뮤니케이션을 하기 위한 도구 중 하나이며, 일대다(one-to-many) 형식의 커뮤니케이션을 사용하며 **브로드캐스트 메세지**를 사용합니다. 브로드캐스트된 메세지는 등록된 모든 observer에게 전달됩니다. 이러한 Notification 메커니즘은 **Observing 객체**와 **Posting 객체**가 서로 직접적으로 알지 못해도 커뮤니케이션을 할 수 있다는게 특징입니다.

#### How to Use Notification

Notification을 사용하기 위해서는 먼저 객체를 notification의 observer로 등록해야 합니다. 특정 이벤트가 발생하거나 발생하려고하면 등록된 객체가 처리하기 위해서입니다. 예를 들면, 윈도우의 크기가 조절됨에 따라 커스텀 뷰를 조절하고 싶으면, 윈도우 객체로부터 발송된 `NSWindowDidResizeNotification`을 관찰하면 됩니다. Notification은 또한 객체간 정보를 전달할 수 있습니다. 이러한 정보들은 **Notification 객체**로 전달됩니다.

일반적인 구현 순서는 아래와 같습니다:

- Notification observer 등록
- Responding method 구현
- Notification을 발송할 객체에 post 메소드 구현

<br>

## The Notification Object

Notification 메커니즘에서 오브젝트 사이에서 보내지는 **notification**은 **객체**로, `NSNotification`의 인스턴스입니다. 이 객체에는 이벤트의 정보가 캡슐화되어있는데, 예를 들면 윈도우를 선택하는 이벤트 또는 네트워크 연결이 닫힐 때의 이벤트가 있습니다. 이벤트가 발생되면, 해당 이벤트를 처리하는 오브젝트는 **notification center**에 **notification**을 발송합니다. 그리고 notification center는 즉시 **등록된 모든 객체**에게 notification을 발송합니다. 등록된 객체는 observer로 notification을 관찰하고 있는 객체들입니다.

`NSNotification` 객체에는 `name`, `object` 그리고 `userInfo`를 포함하고 있습니다. `name`은 `Notification` 객체를 식별하는 태그입니다. Notification은 `name`으로 식별되기 때문에 고유 `name`을 지정해줘야합니다. `object`는 notification 발송할 때, 해당 notification의 observer에게 보내고 싶은 어떤 `object`든지 가능합니다. 보통 notification을 발송한 객체를 사용하며, nil값을 넣으면 사용하지 않는다는 의미입니다. `userInfo`는 Dictionary 타입으로 이벤트와 관련된 정보를 저장합니다.

<br>

#### NSNotification

Notification에 연결되는 등록된 observer에게 브로드캐스트하는 정보를 담고있는 객체

```swift
class NSNotification : NSObject
```

- `var name: NSNotification.Name` - Notification 식별 태그
- `var object: Any?` - Notification 관련 오브젝트
- `var userInfo: [AnyHashable: Any]?` - Notification 관련 정보를 담은 dictionary

<br>

[Notification과 NSNotification의 차이점](https://corykim0829.github.io/ios/NS-classes-in-Swift/#)
{: .notice--info}

## Notification in Observer

예시로 재고 개수가 변하게 되면 notification을 발송해 특정 `UILabel`을 변경하는 코드를 구현해보겠습니다.

#### Add an Observer

먼저 `UIViewController`인 `viewController`에서 observer를 추가하기 위해서 `NotificationCenter`의 `addObserver`메소드를 사용합니다.

```swift
// class NotificationCenter
open func addObserver(_ observer: Any, selector aSelector: Selector, name aName: NSNotification.Name?, object anObject: Any?)
```

Observer로 지정할 객체와 notification을 받은 후 실행될 메소드인 `selector`, name, 그리고 object가 필수 요소입니다.

```swift
// ViewController.swift
NotificationCenter.default.addObserver(
    self, 
    selector: #selector(updateStockLabel(_:)), 
    name: NSNotification.Name(rawValue: "StockDidChangeNotification"),
    object: nil)
```

#### a Selector of an Observer

`addObserver` 메소드 인자인 `selector`에 추가되는 메소드는 notification 메세지에 의해 실행되며, 오직 **Notification type**만을 인자로 받는 메소드입니다. name은 `NSNotification.Name` type으로 notification center를 통해 브로드캐스트 되는 notification을 식별하기 위해서 사용됩니다.

```swift
@objc private func updateStockLabel(_ notification: Notification) {
    guard let changedIndex = notification.userInfo?["changedIndex"] as? Int else { return }
    ...
}
```

위와 같이 인자로 notification을 선언하여, 전달받은 notification object를 사용할 수 있습니다. 여기서 notification의 userInfo를 사용하려면 userInfo 자체도 optional이며 dictionary 값을 가져올 때에도 optional이기 때문에 optional binding은 필수입니다.

<br>

## Notification in a Posting Object

Notification을 발송할 때에는 notification center의 post 메소드를 사용하면 됩니다.

```swift
open func post(name aName: NSNotification.Name, object anObject: Any?, userInfo aUserInfo: [AnyHashable : Any]? = nil)
```

발송할 때 userInfo로 전달되는 dictionary는 optional로 보충적인 정보를 전달할 때 사용된다.

```swift
func postNotification() {
    NotificationCenter.default.post(name: NSNotification.Name(rawValue: "StockDidChangeNotification"),
                                    object: nil,
                                    userInfo: ["changedIndex": changedIndex!,
                                               "numberOfBeverage": changedBeverages.beverages.count])
}
```

Observer 등록할 때 사용했던 동일한 `NSNotification.Name`을 사용하여 notification을 발송해야한다.

<br>

## Using NSNotification.Name with Extension

#### NSNotification.Name

The type used for the name of a notification이라고 공식문서에 명시되어 있으며 **sturct**로 구현되어있습니다.
Swift에서는 Notification 이름은 nested `NSNotification.Name` 타입을 사용합니다.

```swift
struct Name
```

보통 initializer 중 `init(rawValue: String)`을 사용하여 초기화됩니다.

<br>

#### Type Property

`NSNotification.Name`에는 타입 프로퍼티로 구현되어있는 이름들이 많습니다. 예시로, 대표적으로 많이 사용되는 notification의 이름인 `keyboardWillShowNotification`이 있습니다.

```swift
class let keyboardWillShowNotification: NSNotification.Name
```

Apple developer의 공식문서를 참고하면 많은 종류의 타입 프로퍼티를 확인할 수 있습니다.

<br>

#### Typical Uses

Notification observer를 커스텀으로 **추가**하고, notification을 **발송**할 때 필요한 `NSNotifiacion.Name`은 다음과 같이 구현할 수 있습니다.

```swift
NSNotification.Name(rawValue: "StockDidChangeNotification")
```

하지만 이렇게  `NSNotification.Name`을 매번 초기화해서 사용한다면,  **Notification observer** 등록, **notification post 메소드**, 그리고 notification observer를 제거하는 **removeObserver** 메소드 까지 총 3번을 사용하게 됩니다. 매번 String 값을 사용하여 초기화 한다면 하드코딩이 되며, 실수를 할 여지가 다분합니다. String 값을 따로 상수로 만든다고 하여도 observer와 poster 객체가 하나의 String 상수를 사용하는 일은 번거롭습니다. 그래서 Extension을 사용하면 하드코딩도 피하면서 코드로 더욱 쉽게 접근하는 방법이 있습니다.

<br>

#### Extension

```swift
extension NSNotification.Name {
    static let StockNumberDidChange = NSNotification.Name(rawValue: "StockNumberDidChangeNotifiaction")
}
```

`NSNotification.Name`을 확장하여 notification의 식별자로 사용할 이름을 static 상수로 만들어 사용하면 편리하게 사용할 수 있습니다. static으로 만들어진 상수는 우리가 사용하는 method에서 자동완성으로 호출되기 때문에 매우 편리하게 사용 가능합니다.

```swift
// ViewController - addObserver
NotificationCenter.default.addObserver(
    self, 
    selector: #selector(updateStockLabel(_:)),
    name: .StockNumberDidChange, object: nil)
```

```swift
NotificationCenter.default.post(
    name: .StockNumberDidChange,
    object: nil,
    userInfo: ["changedIndex": changedIndex!,
               "numberOfBeverage": changedBeverages.beverages.count])
```

```swift
deinit {
    NotificationCenter.default.removeObserver(
        self,
        name: .StockNumberDidChange,
        object: nil)
}
```

이렇게 extension을 활용하면, Refactoring을 할 때에도 훨씬 수월합니다.

<br>

#### References

- [Communicating with Objects - Apple Developer Document](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW7)
- [Notification in Swift](https://medium.com/@dmytro.anokhin/notification-in-swift-d47f641282fa)
- [NSNotification](https://developer.apple.com/documentation/foundation/nsnotification)