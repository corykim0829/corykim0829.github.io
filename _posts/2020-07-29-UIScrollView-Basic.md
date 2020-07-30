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
lastmod: 2020-07-29 16:25:00
sitemap :
  changefreq : daily
  priority : 1.0
---

스크롤 뷰를 스토리보드에 오토 레이아웃을 적용하여 구현해보자.

<br>

### 들어가며

스크롤 뷰는 개인적으로 사용할 때마다 헷갈리는 UIKit 중 하나입니다. 스크롤 뷰를 상속한 UIKit 요소들이 꽤 있기 때문에 스크롤 뷰는 사용하지 않을 수 없는 친구입니다. 이러한 스크롤 뷰를 **스토리보드**에서 오토 레이아웃과 함께 사용하는 방법을 정리해보려고 합니다.

<br>

#### UIScrollView

`UIScrollView`는 `UITableView`, `UITextView`, `UICollecctionView`을 포함하여 몇가지 UIKit의 클래스들의 superclass입니다. 스크롤 뷰의 핵심은 스크롤되는 **content**와 보여지는 **frame**으로 나뉘어진다는 겁니다. 스크롤 되는 전체 content는 많지만 frame에 잘려서 스크롤 되는 것처럼 보여지는 겁니다. 이 content와 frame 키워드를 잘 기억해주세요.

<br>

#### 개발 환경

- Xcode 11.6
- Swift 5.3

## Vertical ScrollView in Storyboard

먼저 **세로**로 스크롤 할 수 있는 스크롤 뷰를 구현해보겠습니다.

뷰컨트롤러의 뷰와 스크롤뷰를 구분하기 위해 뷰컨트롤러의 뷰의 background color를 주황색으로 변경하였습니다. 이 뷰컨트롤러의 이름은 `VerticalViewController`라고 하겠습니다.

<p align="center">
  <img src="/assets/images/scrollview/1.png" width="300px">
</p>

<br>

이제 `VerticalViewController`에 **스크롤 뷰**를 추가하여 클릭한 뒤 스토리보드 오른쪽 아래에 있는 `Add New Constraints` 버튼을 눌러 스크롤 뷰의 auto layout을 줍니다. 아래 스크린샷과 같이 스크롤 뷰의 leading, top, trailing, bottom anchor를 스크롤 뷰의 superView의 leading, top, trailing, bottom anchor와 일치하게 constraint를 주고 constant는 0으로 잡아줍니다.

스크롤 뷰가 스토리보드에서 자동으로 늘어나지 않으면, 직접 스크롤 뷰를 늘려주세요.
{: .notice--warning}

위와 같이 constraint를 잡아주면 스크롤 뷰의 `Frame Layout Guide`와 `Content Layout Guide` 중  `Frame Layout Guide`의 constraint를 설정한 것입니다.

<p align="center">
  <img src="/assets/images/scrollview/2.png" width="520px">
</p>

<br>

위에서 스크롤 뷰의 leading, top, trailing, bottom에 constraint를 모두 줬는데도 다음과 같이 경고가 뜨게 됩니다. 그 이유는 바로 스크롤 뷰의 **content**의 사이즈가 정해져 있지 않기 때문에 발생하는 경고입니다.

<p align="center">
  <img src="/assets/images/scrollview/3.png" width="540px">
</p>

<br>

빨간 화살표 클릭하면 다음과 같은 경고가 뜨게 됩니다.

<br>

<p align="center">
  <img src="/assets/images/scrollview/warning.png" width="400px">
</p>

```
Has ambiguous scrollable content width/height
```

스크롤 뷰의 content의 사이즈가 모호하다는 경고입니다.


<br>

스크롤 뷰에서 가장 중요한 것은 content의 사이즈인 **width와 height**가 정해져 있어야 합니다. **frame**이 정해져 있어도 실제로 화면에 보여줄 content가 없다면 의미가 없기 때문입니다. 그래서 **content**를 담을 `contentView`라는 이름을 가진 `UIView`를 하나 만들어서 스크롤 뷰에 추가합니다.

<p align="center">
  <img src="/assets/images/scrollview/4.png" width="540px">
</p>
<br>

추가된 `contentView`를 스크롤 뷰의 `Content Layout Guide`에 오토 레이아웃을 사용해 꽉 채워줍니다.

`contentView`를 `control`을 누른상태로 `Content Layout Guide`에 드래그하여 오토 레이아웃을 줄 수 있습니다. 이와 같은 방식으로 오토 레이아웃을 주면 `Leading Space to Content Layout Guide`라는 옵션을 볼 수 있는데, `contentView`의 leading anchor와 `Content Layout Guide`의 leading anchor를 일치하게 하면서 현재 스토리보드에서 보여지는 두 anchor의 차이만큼 거리(spacing)을 주겠다는 의미입니다.

따라서 먼저 `contentView`를 수동으로 늘려서 constraint를 줘도 되고, 먼저 constraint를 주고 XCode의 오른쪽 inspector 창에서 `contentView`의 leading, top, trailing, bottom anchor constraint의 constant를 0으로 잡아줘도 됩니다. 저는 후자를 사용하였습니다.

<p align="center">
  <img src="/assets/images/scrollview/5-1.png" width="540px">
</p>

<br>

<p align="center">
  <img src="/assets/images/scrollview/5-2.png" width="540px">
</p>

이렇게 constraint를 잡아줘도 `contentView`의 실질적인 width와 height가 명확하게 없기 때문에 경고가 뜨게 됩니다. 

<br>

### Content View 사이즈 잡아주기

세로로 스크롤을 하기 위해서는 **가로는 고정**되고 **세로는 동적**으로 늘어날 수 있어야합니다. 

그래서 먼저 `contentView`의 width를 스크롤 뷰의 `Frame Layout Guide`와 동일하도록 지정해야합니다. `contentView`를 `control`을 클릭한 상태로 드래그하여 `Equal widths` 옵션을 선택해주고 `multiplier`를 1로 지정해줍니다. 여기서도 마찬가지로 스토리보드에 보여지는 상태 그대로 `Equal widths`가 적용되기 때문에 처음에는 1:1 비율이 아니기에 inspector 창에서 바꿔줘야하는 겁니다.

<p align="center">
  <img src="/assets/images/scrollview/6.png" width="400px">
</p>

<br>

Height는 `contentView`를 **스크롤 뷰에 꽉 차게** 늘린 후(이전 단계에서 늘렸다면 그대로 하시면 됩니다.)에 `contentView`를 control을 누른 상태로 **자기 자신**에게 드래그하여 height를 지정해줍니다. 이렇게 되면 height는 현재 스토리보드에 보여지는 `contentView`의 height인 고정값으로 지정되게 됩니다.

<p align="center">
  <img src="/assets/images/scrollview/7.png" width="520px">
</p>
고정값을 주게 되면 스크롤에 제한이 생기게 됩니다. 따라서 `contentView`안에 더 들어올 수 있는 UI 요소들에 고려하여 `contentView`의 height가 동적으로 변경될 수 있게 해야합니다. 

따라서 `contentView`의 height constraint의 priority를 250으로 낮춰줍니다. 이렇게 되면 `contentView`의 height는 완전히 고정된 값이 아니게 되어 동적으로 바뀔 수 있습니다. 

<p align="center">
  <img src="/assets/images/scrollview/8.png" width="320px">
</p>

<br>

사이즈까지 정상적으로 constraint를 줬다면, 오토레이아웃이 정상적으로 적용되고, 스토리보드에 경고가 뜨지 않는 것을 확인할 수 있습니다.

<p align="center">
  <img src="/assets/images/scrollview/9.png" width="520px">
</p>
<br>


이제 여기에 `UIStackView`를 하나 추가하여 동적으로 스크롤 뷰의 content가 잘 늘어나는지 확인해보도록 하겠습니다. 스택 뷰를 `contentView`에 꽉 차도록 오토레이아웃을 준 후에 `@IBOutlet`으로 스택 뷰를 프로퍼티로 만들어서 아래와 같이 코드를 작성합니다. 일정한 랜덤의 크기와 랜덤한 색의 뷰를 생성하여 스택 뷰에 추가해주는 코드입니다.

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

`UIScrollView`는 처음에 사용해보기 까다로운 친구입니다. 하지만 content와 frame 개념을 잘 숙지하면 기본적인 스크롤 뷰를 만드는 것에는 큰 어려움이 없는 것 같습니다. 하지만 스크롤 뷰는 정말 많이 사용되고 애니메이션에서도 많이 사용되기 때문에 아직 공부해야할 부분이 많은 것 같습니다. 읽어주셔서 감사합니다. 피드백은 언제나 환영입니다. 😀

<br>

#### References

- [UIScrollView](https://developer.apple.com/documentation/uikit/uiscrollview)