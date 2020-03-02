---
title: "[iOS] The Observer Pattern - Notification"
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
---

iOS의 Observer Pattern과 Notification에 대해서 알아보자

<br>

## Observer

**Observer 디자인 패턴**은 오브젝트와 오브젝트 사이에서 일대다(one-to-many) 관계의 의존성을 정의한다. 한쪽 오브젝트의 상태가 바뀌면 의존성으로 연결된 모든 오브젝트에게 알려 자동적으로 업데이트된다. Observer 패턴은 본질적으로 **발행-및-구독(publish-and-subscribe)** 모델이며, **주체(Subject)**와 주체의 observer가 느슨하게 결합되어있다. 여기서 주체는 발행을 하는 오브젝트이며 주체의 observer는 주체가 발행하는 것을 구독하여 변경사항을 받는 오브젝트이다. 즉, **Observing 오브젝트**와 **observed 오브젝트** 사이에서 의사소통이 이루어지며 두 오브젝트는 서로를 많이 알 필요가 없다.

Publisher-Subscriber pattern에서는, 메세지를 보내는 사람을 publisher라고 부르고, 메세지를 직접적으로 수취인에게 보내지지 않도록 프로그래밍 하는데, 여기서 수취인은 subscriber라고 부른다.
{: .notice--warning}

## Notification

Cocoa의 notification 메커니즘은 Observer 패턴 기반으로 **일대다(one-to-many) 브로드캐스팅 메세지**를 구현한다. 프로그램의 오브젝트는 `하나 이상의 notification`의 observer로 이루어진 목록에 자신 또는 다른 오브젝트를 추가하며, 각 notification은 전역 String(notification name)으로 식별된다. 다른 오브젝트에 notification을 보내고 싶은 오브젝트는 **notification 오브젝트**를 만들어 **notification center**로 발송(post)한다. notification center는 해당 notification의 observer를 결정하고 메세지를 통해 notification을 전송한다. 이 때, notification 메세지에 의해 실행되는 **메소드**는 정해진 **하나의 파라미터**만 받을 수 있다. 파라미터는 **notification 오브젝트**로 다음과 같은 정보들을 담고있다: notification name, observed object, 그리고 보충 정보를 담는 dictionary

<br>

<p align="center">
  <img src="https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/Art/notificationcenter.gif">
</p>

<br>

Notification을 발송하는 것은 동기적 절차이다. 발송하는 오브젝트는 notification center가 모든 observer에게 notification을 브로드캐스트하기 전까지 통제권을 갖지 못한다. 비동기 처리에서는, notification을 notification 큐에 넣을 수 있는데, 제어는 즉시 발송하는 오브젝트로 돌아가고 notification center는 큐의 가장 위에 도달하는 notification을 브로드캐스트한다.

<br>

<p align="center">
  <img src="https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/Art/notificationqueue.gif">
</p>

<br>

#### Uses and Limitations

여러가지 이유로 notification을 사용할 수 있다. 예를 들어, 프로그램에서 특정 이벤트에 기반하여 사용자에게 보여지는 인터페이스의 정보를 바꾸기 위해 notification을 브로드캐스트할 수 있다. 아니면 notification을 사용하여 문서 창이 닫히기 전 문서의 상태를 안전하게 저장하기 위한 방법으로 사용할 수 있다. 일반적인 목적은 프로그램의 다른 오브젝트에게 이벤트를 알려서 적절한 반응을 하기 위함이다.

하지만 notification을 받는 오브젝트는 이벤트가 발생한 이후에만 반응할 수 있다. 이는 delegation과 아주 큰 차이점이다. delegate는 delegating object로 부터 오는 작업을 거부하거나 수정할 기회를 갖는다. 반면에 observing object는 observed object로부터 오는 notification 작업에 직접적인 영향을 줄 수 없다.

notification class에는 `NSNotification`(notification object), `NotificationCenter`(notification 발송과 observer를 추가하기 위해서), `NotificationQueue`(notification을 큐에 넣기위해서), 그리고 `DistributedNotificationCenter`

<br>

> NSNotification

Notification에 연결되는 등록된 observer에게 브로드캐스트하는 정보를 담고있는 객체

> Notification

Notification center를 통해 등록된 모든 observer에게 정보를 브로드캐스트하는 컨테이너입니다. 

> NotificationCenter

notification 전송 메커니즘으로 등록된 observer에게 정보를 브로드캐스트할 수 있게 한다. 

모든 실행 중인 앱은 각각의 default notification center를 가지고 있다.

> NotificationQueue

notification center의 버퍼

> DistributedNotificationCenter

notification 전송 메커니즘으로 task boundaries를 넘어 notification을 브로드캐스트할 수 있다.

<br>

#### References

- [Cocoa Design Pattern: Observer - Apple Developer Document](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/CocoaDesignPatterns/CocoaDesignPatterns.html#//apple_ref/doc/uid/TP40002974-CH6-SW20)
- [Observer vs Pub-Sub pattern](https://hackernoon.com/observer-vs-pub-sub-pattern-50d3b27f838c)
- Notification classes in Apple Developer
  - [NSNotification](https://developer.apple.com/documentation/foundation/nsnotification)
  - [NotificationCenter](https://developer.apple.com/documentation/foundation/notificationcenter)
  - [NotificationQueue](https://developer.apple.com/documentation/foundation/notificationqueue)
  - [DistributedNotificationCenter](https://developer.apple.com/documentation/foundation/distributednotificationcenter)