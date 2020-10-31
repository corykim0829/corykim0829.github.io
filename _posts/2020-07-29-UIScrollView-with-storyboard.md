---
title: "[iOS] UIScrollView 스토리보드에서 오토 레이아웃으로 구현하기"
header:
  teaser: /assets/images/ios-posts2.jpeg
  overlay_image: /assets/images/ios-posts2.jpeg
  overlay_filter: 0.2
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - iOS
tags:
  - UIScrollView
  - UIScrollView Frame Layout Guide
  - UIScrollView Content Layout Guide
  - UIScrollView Storyboard
  - UIScrollView 스토리보드
  - UIScrollView autolayout
date: 2020-07-29 16:25:00
lastmod: 2020-10-31 22:35:00
sitemap :
  changefreq : daily
  priority : 1.0
---

스크롤 뷰를 스토리보드에 오토 레이아웃을 적용하여 구현해보자.

<br>

### 들어가며

`UIScrollView`는 개인적으로 사용할 때마다 헷갈리는 UIKit 클래스 중 하나입니다. 스크롤 뷰를 상속한 UIKit 클래스들이 꽤 있기 때문에 스크롤 뷰는 꽤나 중요한 친구입니다. 스크롤 뷰를 **스토리보드**에서 사용하려면 항상 빨간 경고를 만나게 됩니다. 이 스크롤 뷰를 스토리보드에서 오토 레이아웃과 함께 사용하는 방법을 정리해보았습니다.

<br>

#### UIScrollView

`UIScrollView`는 `UITableView`, `UITextView`, `UICollecctionView`을 포함하여 몇가지 UIKit 클래스들의 superclass입니다. 

스크롤 뷰는 스크롤되는 뷰들을 담고 있는 **Content Layout**과 화면에 보여지는 **Frame Layout**으로 나뉘어 집니다. **Content layout**에는 스크롤 되는 전체 content를 가지고 있고, **Frame layout**은 잘려서 보여지기 때문에 스크롤 해야 content layout의 다른 contents를 볼 수 있습니다. 이 두개의 키워드를 알고 보시면 더욱 이해하기 쉽습니다.

<br>

#### 개발 환경

- Xcode 11.6
- Swift 5.3

<br>

## Vertical ScrollView in Storyboard

이 포스트에서는 **세로**로 스크롤 할 수 있는 스크롤 뷰를 구현해보겠습니다. 가로는 세로가 익숙해지면 응용하여 충분히 구현할 수 있습니다.

과정은 크게 다음과 같습니다.

- 스크롤 뷰 오토레이아웃 잡아주기
- ContentView 추가하기
- Content View 사이즈 잡아주기
- Frame Layout 잡아주기
- Width/Height 잡아주기

<br>

### 스크롤 뷰 오토레이아웃 잡아주기

뷰컨트롤러의 뷰와 스크롤뷰를 구분하기 위해 뷰컨트롤러의 뷰의 background color를 주황색으로 변경하였습니다. 이 뷰컨트롤러의 이름은 `VerticalViewController`라고 하겠습니다.

<p align="center">
  <img src="/assets/images/scrollview/1.png" width="300px">
</p>
<br>

<br>

이제 `VerticalViewController`에 **스크롤 뷰**를 추가하여 클릭한 뒤 스토리보드 오른쪽 아래에 있는 `Add New Constraints` 버튼을 눌러 스크롤 뷰에 auto layout을 줍니다. 아래 스크린샷과 같이 스크롤 뷰의 leading, top, trailing, bottom anchor를 스크롤 뷰의 superView의 leading, top, trailing, bottom anchor와 일치하게 constraint를 주고 constant는 0으로 잡아줍니다.

스크롤 뷰가 스토리보드에서 자동으로 늘어나지 않으면, 직접 스크롤 뷰를 늘려주세요.
{: .notice--warning}

<p align="center">
  <img src="/assets/images/scrollview/2.png" width="520px">
</p>
위와 같이 constraint를 잡아주면 스크롤 뷰의 `Frame Layout Guide`와 `Content Layout Guide` 중  `Frame Layout Guide`의 constraint를 설정한 것입니다.

<br>

<br>

위에서 스크롤 뷰의 leading, top, trailing, bottom에 constraint를 모두 줬는데도 다음과 같이 경고가 뜨게 됩니다. 그 이유는 바로 스크롤 뷰의 **content**의 사이즈가 정해져 있지 않기 때문에 발생하는 경고입니다.

<p align="center">
  <img src="/assets/images/scrollview/3.png" width="540px">
</p>
<br>

<br>

빨간 화살표 클릭하면 다음과 같은 경고가 뜨게 됩니다.

<p align="center">
  <img src="/assets/images/scrollview/warning.png" width="400px">
</p>

```
Has ambiguous scrollable content width/height
```

스크롤 뷰의 content의 사이즈가 모호하다는 경고입니다.

<br>

<br>

### ContentView 추가하기

이제 스크롤 뷰의 **content를 담을 뷰**가 필요합니다. 그리고 이 뷰는 실질적인 사이즈가 필요합니다. 그래서 content의 **width와 height**가 필요합니다. 스크롤 뷰의 **frame**이 정해져 있어도 실제로 화면에 보여줄 content가 없다면 의미가 없기 때문입니다. 그래서 **content**를 담을 `UIView`를 하나 스크롤 뷰에 추가하고 이름을  `contentView`라고 하겠습니다.

<p align="center">
  <img src="/assets/images/scrollview/4.png" width="540px">
</p>
<br>

<br>

추가된 `contentView`를 스크롤 뷰의 `Content Layout Guide`에 오토 레이아웃을 사용해 **꽉 채워줍니다**.

`contentView`를 `control`을 누른상태로 `Content Layout Guide`에 드래그하여 오토 레이아웃을 줄 수 있습니다. 이 방법으로 leading, top, trailing, bottom anchor 모두 constraint를 적용해줍니다.

이와 같은 방식으로 오토 레이아웃을 주면 `Leading Space to Content Layout Guide`라는 옵션을 볼 수 있는데, `contentView`의 leading anchor와 `Content Layout Guide`의 leading anchor를 일치하게 하면서 현재 스토리보드에서 보여지는 두 anchor의 차이만큼 거리(spacing)을 주겠다는 의미입니다. 이렇게 constraint를 적용한 뒤에 나중에 space 값을 바꿔줘도 되고 아니면 스토리보드에 있는 뷰를 원하는 크기로 맞춘 다음에 드래그하여 constraint를 주게되면 constant값을 추가로 수정할 필요가 없습니다.

<p align="center">
  <img src="/assets/images/scrollview/5-1.png" width="540px">
</p>

<br>

<p align="center">
  <img src="/assets/images/scrollview/5-2.png" width="540px">
</p>
<br>

<br>

이렇게 constraint를 잡아줘도 `contentView`의 실질적인 width와 height가 명확하게 없기 때문에 경고가 뜨게 됩니다. 

<br>

### Content View 사이즈 잡아주기

세로로 스크롤을 하기 위해서는 content view의 **가로는 고정**되고 **세로는 동적**으로 변할 수 있어야합니다. 

그래서 먼저 `contentView`의 가로를 스크롤 뷰의 `Frame Layout Guide`와 동일하도록 지정해야합니다. `contentView`를 `control`을 클릭한 상태로 드래그하여 `Equal widths` 옵션을 선택해주고 `multiplier`를 1로 지정해줍니다. 여기서도 마찬가지로 스토리보드에 보여지는 상태 그대로 `Equal widths`가 적용되기 때문에 처음에는 1:1 비율이 아니기에 inspector 창에서 바꿔줘야합니다.

<p align="center">
  <img src="/assets/images/scrollview/6.png" width="400px">
</p>

<br>

`contentView` 높이는 `contentView`를 **스크롤 뷰에 꽉 차게** 늘린 후, `contentView`를 control을 누른 상태로 **자기 자신**에게 드래그하여 높이를 지정해줍니다. 이렇게 되면 `contentView`의 높이는 현재 스토리보드에 보여지는 `contentView`의 높이로 **고정값**이 생기게 됩니다.

<p align="center">
  <img src="/assets/images/scrollview/7.png" width="520px">
</p>
<br>

<br>

하지만 이렇게 **고정값**을 주게 되면 스크롤에 제한이 생기게 됩니다. 따라서 `contentView`안에 더 들어올 수 있는 UI 요소들에 고려하여 `contentView`의 높이가 동적으로 변경될 수 있게 하기 위해 **priority**를 낮춰줘야합니다.

따라서 `contentView`의 **height constraint**의 **priority**를 250으로 낮춰줍니다. 이렇게 되면 `contentView`의 높이는 완전히 고정된 값이 아니게 되어 동적으로 바뀔 수 있습니다. 

<p align="center">
  <img src="/assets/images/scrollview/8.png" width="320px">
</p>

<br>

사이즈까지 정상적으로 constraint를 줬다면, 오토레이아웃이 정상적으로 적용되고, 스토리보드에 경고가 뜨지 않는 것을 확인할 수 있습니다.

<p align="center">
  <img src="/assets/images/scrollview/9.png" width="520px">
</p>
<br>

<br>


스크롤 뷰의 `contentView`에 `UIStackView`를 하나 추가하여 동적으로 `contentView`의 높이가 바뀌는지 확인해보도록 하겠습니다. 스택 뷰를 `contentView`에 꽉 차도록 오토레이아웃을 준 후에 아래와 같이 코드를 작성해줬습니다. 일정한 랜덤의 크기와 랜덤한 색의 뷰를 생성하여 스택 뷰에 추가해주는 코드입니다.

```swift
import UIKit

class VerticalViewController: UIViewController {

    @IBOutlet weak var verticalStackView: UIStackView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        configureStackView()
    }
    
    private func configureStackView() {
        for _ in 0..<10 {
            let dummyView = randomColoredView()
            verticalStackView.addArrangedSubview(dummyView)
        }
    }
    
    // 랜덤 색상, 100~400 height를 가진 뷰 생성 함수
    private func randomColoredView() -> UIView {
        let view = UIView()
        view.backgroundColor = UIColor(
            displayP3Red: 1.0,
            green: .random(in: 0...1),
            blue: .random(in: 0...1),
            alpha: .random(in: 0...1))
        view.translatesAutoresizingMaskIntoConstraints = false
        view.heightAnchor.constraint(equalToConstant: .random(in: 100...400)).isActive = true
        return view
    }
}
```

<br>

### 구현 화면

추가된 10개의 View가 모두 스택 뷰로 들어가 정상적으로 스크롤이 되는 것을 확인할 수 있습니다.

<p align="center">
  <img src="/assets/images/scrollview/vertical.gif" width="320px">
</p>
<br>

### 마치며

`UIScrollView`는 처음에는 사용해보기 까다로운 친구입니다. 하지만 content layout과 frame layout 개념을 잘 숙지하면 기본적인 스크롤 뷰를 만드는 것에는 큰 어려움이 없는 것 같습니다. 읽어주셔서 감사합니다. 피드백은 언제나 환영입니다. 😀

<br>

#### References

- [UIScrollView](https://developer.apple.com/documentation/uikit/uiscrollview)