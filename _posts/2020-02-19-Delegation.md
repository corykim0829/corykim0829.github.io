---
title: "[iOS] Delegation 톺아보기"
header:
  teaser: /assets/images/ios-posts2.jpeg
  overlay_image: /assets/images/ios-posts2.jpeg
  overlay_filter: 0.2
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - iOS
tags:
  - iOS
  - Delegation
date: 2020-02-19 12:00:00
lastmod: 2020-02-19 12:00:00
sitemap :
  changefreq : daily
  priority : 1.0
---

iOS의 Design pattern, Delegation은 무엇인가?

<br>

## Delegation

Delegate의 사전적 의미는 "특정한 일, 의무, 권리 등을 나 대신 할 수 있는 사람에게 주는 것"입니다. Delegation의 사전적 의미는 "다른 사람을 위해 하는 행동을 부여하는 것"이라고 등재되어있습니다.

사전적 의미와 비슷하게, Delegation은 하나의 객체가 프로그램에서 다른 객체를 **대신하여 동작**하거나 다른 객체와 함께 동작하는 간단하며 강력한 패턴입니다. Delegation을 사용할 때는 보통 두개의 객체가 필요합니다. 위임을 할 수 있는 객체인 **Delegating Object**, 그리고 위임받아 일을 대신해서 처리할 **Delegate** 객체가 필요합니다. 이러한 Delegation은 **특정 행동에 반응**하거나 해당 소스를 알 필요 없이 **외부 소스로부터 data를 가져올 때** 사용될 수 있습니다.

- **Delegating Object** : 직역하면 위임 객체이며, 위임할 수 있는 delegate 프로퍼티를 가지고 있습니다.
- **Delegate** : 대리자(Delegate)는 protocol로 캡슐화된 작업들을 채택하며, Delegating Object의 일을 대신 처리할 수 있습니다.

**Delegating object**는 다른 **delegate 객체**에 참조를 유지하고 적절한 때에 **delegate 객체**에게 **메세지**를 보낼 수 있습니다. 메세지는 **Delegating object**가 처리할 예정이거나 처리된 이벤트를 전달합니다. **delegate**는 보여지는 부분, 앱 안의 자신 또는 다른 객체들의 상태를 업데이트하여 메세지에 응답 할 수 있습니다. 때로는 값을 리턴할 수 있는데, 그 값은 걸쳐진 이벤트가 어떻게 처리되는 지에 영향을 주기도 합니다. Delegation의 가장 큰 장점은 하나의 중심 객체에서 여러 객체의 동작을 쉽게 커스터마이징할 수 있게 해준다는 것입니다.

<br>

#### Delegation and the Cocoa Frameworks

**Delegating object**는 보통 프레임워크 객체이고, **delegate**는 보통 커스텀 컨트롤러 객체입니다. Swift의 ARC와 같은 메모리가 관리되는 환경에서는, 강한 순환 참조를 방지하기 위해 **delegating object는 약한 참조를 delegate에 유지합니다.** garbage-collected 환경에서는, receiver는 강한 참조를 delegate에 유지한다고 합니다. Delegation 예제는 Foundation, UIKit, AppKit, 그리고 다른 Cocoa, Cocoa Touch 프레임워크에 많이 있습니다.

**Delegating object**의 예시로 AppKit 프레임워크의 `NSWindow` 클래스의 인스턴스라고 하겠습니다. `NSWindow`는 `windowShouldClose:`라는 메소드를 가지고 있는 프로토콜(`NSWindowDelegate`)을 선언합니다. 사용자가 윈도우의 닫기를 클릭하면, 윈도우 객체는 `windowShouldClose:` 를 **delegate**에 보내서 윈도우 객체의 닫기를 확인하도록 요청합니다. 보통 커스텀 컨트롤러인 delegate는 Boolean 값을 리턴하여 윈도우 객체의 동작을 대신하여 처리합니다. 프로토콜에 있는 `windowShouldClose:` 안에 커스텀하게 구현을 하여 현재 상황에 맞게 값을 리턴할 수 있습니다.

- Delegating Object : 프레임워크 객체
- Delegate : 커스텀 컨트롤러 객체

<img src="https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Art/delegation_2x.png">

#### Delegation and Notification

Cocoa framework 클래스의 대부분의 **delegate**는 delegating object가 작성한 notification의 **observer**로 자동적으로 등록이됩니다. **delegate**는 **특정 notification 메세지**를 받기 위해 framework 클래스에 의해 선언된 notification을 처리하는 notification 메소드를 구현하면 됩니다. 위 그림에서, Delegating object인 window 객체는 `NSWindowWillCloseNotification`을 observer에게 보내는 것은 결국 `windowShouldClose` 메세지를 delegate에게 보내는 원리입니다.

#### Data Source

종종 UIKit에서 정의된 클래스의 protocol을 사용하다보면 delegate뿐 아니라 data source를 확인할 수 있습니다.

Data source는 delegate와 거의 동일하다고 볼 수 있습니다. 차이점은 delegating object와의 관계입니다. UI 제어를 위임받는게 아닌, **data 제어**를 위임받습니다. Delegating object 의 예로 보통 view 객체인 `UITableView`의 객체가 있는데, `UITableView` 객체는 **data source**와의 참조를 유지하며 때때로 표시할 data를 요구합니다. Data source도 delegate와 마찬가지로 protocol을 채택해야 하고, 최소한 protocol의 필수 메소드를 구현해야합니다. Data source는 delegating view에 제공하는 모델 객체들의 메모리를 관리합니다.

---

### UIImagePickerControllerDelegate

자주 사용되는 delegation을 예시를 들어보겠습니다. `SecondViewController`를 통해 `UIImagePickerController`를 present하여 사용하려는 예제입니다.

```swift
class SecondViewController: UIViewController, UIImagePickerControllerDelegate, UINavigationControllerDelegate {

    let imagePickerController = UIImagePickerController()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        imagePickerController.delegate = self
    }
  
    func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
        ...
    }
}
```

`UIImagePickerControllerDelegate`을 채택할 때에는 일반적으로 `UIImagePickerController`의 객체의 상위 `UIViewController` 객체가 delegate역할을 하게 됩니다. `UIImagePickerController`의 일을 처리하기 위해서 `UIImagePickerControllerDelegate`, `UINavigationControllerDelegate` 두개의 프로토콜을 채택해야합니다.

`func imagePickerController`는 프로토콜에 정의된 함수로 `UIImagePickerController`의 특정 행동을 처리합니다.

extension을 사용하여 프로토콜 채택과 특정 행동에 대한 처리를 분리할 수도 있습니다.

```swift
class SecondViewController: UIViewController {

    let imagePickerController = UIImagePickerController()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        imagePickerController.delegate = self
    }
}

extension SecondViewController: UIImagePickerControllerDelegate, UINavigationControllerDelegate {
     func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
        ...
    }
}
```



<br>

#### UIImagePickerController

`UIImagePickerController`의 구현 부분을 살펴보면 **강한 참조 순환**을 방지하기 위해서 delegate 프로퍼티를 weak reference로 선언되어 있습니다.

```swift
open class UIImagePickerController : UINavigationController, NSCoding {
  
    weak open var delegate: (UIImagePickerControllerDelegate & UINavigationControllerDelegate)?
}
```

<br>

#### UIImagePickerControllerDelegate

사용자가 UIImagePickerController 객체를 통해 미디어를 선택했을 때 실행되는 `imagePickerController(_:didFinishPickingMediaWithInfo:)` 메소드와 미디어 선택이 취소 됐을 때 실행되는 `imagePickerControllerDidCancel(_:)` 메소드입니다. 이와 같이 특정 행동에 대해서 delegate 객체가 처리할 수 있도록 프로토콜에 선언되어있습니다.

```swift
public protocol UIImagePickerControllerDelegate : NSObjectProtocol {
    
    @available(iOS 2.0, *)
    optional func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any])

    @available(iOS 2.0, *)
    optional func imagePickerControllerDidCancel(_ picker: UIImagePickerController)
}
```

<br>

### Delegation 구조

Delegating object와 delegate가 상당히 헷갈리는 부분이기도 해서 직접 그림으로 정리를 해보았습니다.

<img src="/assets/images/delegation-pattern-image-picker.png" width = "800px">

- Delegating Object는 `delegate` 변수를 가지고 있어 객체를 위임할 수 있다.
- `delegate` 변수로 지정받는 객체는 delegate이다. 
- Delegate 객체는 delegating object에서 보낸 메세지나 행동에 응답한다.

<br>

#### 피드백 주신 고마운 분들

- Lena

<br>

#### References

- [Swift 5.2 - Protocols](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID276)
- [Delegation - Apple Developer](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html)
- [Understanding Delegates and Delegation in Swift 4](https://www.appcoda.com/swift-delegate/)
- [Garbage Collection vs Automatic Reference Counting](https://medium.com/computed-comparisons/garbage-collection-vs-automatic-reference-counting-a420bd4c7c81)