---
title: "[iOS] UIScrollView 오토레이아웃과 함께 적용해보기"
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

스크롤 뷰를 스토리보드에 오토레이아웃을 적용하여 구현해보자.

<br>

### 들어가며

스크롤 뷰는 개인적으로 사용할 때마다 바로바로 구현이 안되는 UIKit 중 하나입니다. 하지만 스크롤뷰는 사용하지 않을 수 없는 친구이기 때문에, 스크롤뷰를 오토레이아웃과 함께 사용하는 방법을 정리해보려고 합니다.



#### UIScrollView

`UIScrollView`는 `UITableView`, `UITextView`, `UICollecctionView`을 포함하여 몇가지 UIKit의 클래스들의 superclass입니다. 스크롤 뷰의 핵심은 스크롤되는 content와 보여지는 frame으로 나뉘어진다는 겁니다. 스크롤 되는 전체 content는 많지만 frame에 잘려서 스크롤 되는 것처럼 보여지는 겁니다.



#### 개발 환경

- Xcode 11.6
- Swift 5.3

### Vertical ScrollView in Storyboard

전체화면을 세로로 스크롤 할 수 있도록 구현하도록

스크롤 뷰를 추가하기 전, 뷰컨트롤러의 뷰와 스크롤뷰를 구분하기 위해 뷰컨트롤러의 뷰의 background color를 주황색으로 변경하였습니다.

<p align="center">
  <img src="/assets/images/scrollview/1.png" width="300px">
</p>

<br>

이제 스크롤 뷰를 추가하여 스크롤뷰의 auto layout을 superView에 leading, top, trailing, bottom anchor를 맞춰주고 constant를 0으로 잡아줍니다.

위와 같이 constraint를 잡아주면, 자동으로 스크롤 뷰의 **frame layout guide**가 스크롤 뷰의 super view에 constraint가 잡혀집니다.

<p align="center">
  <img src="/assets/images/scrollview/2.png" width="520px">
</p>

<br>

스크롤 뷰의 leading, top, trailing, bottom constraint를 다 줬는데도 다음과 같이 경고가 뜨게 됩니다. 그 이유는 바로 스크롤 뷰의 content의 사이즈가 정해져 있지 않기 때문에 발생하는 경고입니다.

<p align="center">
  <img src="/assets/images/scrollview/3.png" width="540px">
</p>

<br>

스크롤 뷰에서 가장 중요한 것은 content의 사이즈인 **width와 height**가 정해져 있어야한다는 것입니다. 그러기 위해서 content를 담을 `contentView`라는 이름을 가진 `UIView`를 하나 만들어서 스크롤 뷰에 추가할 것입니다.

<p align="center">
  <img src="/assets/images/scrollview/4.png" width="540px">
</p>

<br>

추가된 `contentView`는 스크롤 뷰의 `Content Layout Guide`에 leading, top, trailing, bottom anchor를 constant 0으로 잡아줍니다.

<p align="center">
  <img src="/assets/images/scrollview/5-1.png" width="400px"> <img src="/assets/images/scrollview/5-2.png" width="400px">
</p>

이렇게 Constraint를 잡아줘도 `contentView`의 실질적인 width와 height가 명확하게 없기 때문에 경고가 뜨게 됩니다. 

#### Content View 사이즈 잡아주기

세로로 스크롤을 하기 위해서는 가로는 고정되고 세로는 동적으로 늘어날 수 있어야합니다.

먼저 contentView의 width를 스크롤 뷰의 `Frame Layout Guide`와 동일하도록 지정한다.

마찬가지로 content view를 control로 클릭하여 드래그하여 `Equal widths`를 multiplier를 1로 지정해줍니다.

<p align="center">
  <img src="/assets/images/scrollview/6.png" width="320px">
</p>

<br>

Height는 content view를 **스크롤 뷰에 꽉 차게** 늘린 후에 content view를 control을 누른 상태로 클릭하여 **자기 자신**에게 드래그하여 height를 지정해줍니다. 

<p align="center">
  <img src="/assets/images/scrollview/7.png" width="520px">
</p>

그 뒤에 `contentView`의 height constraint의 priority를 250으로 낮춰줍니다.

<p align="center">
  <img src="/assets/images/scrollview/8.png" width="320px">
</p>

<br>

오토레이아웃이 정상적으로 적용되고, 이제 경고가 뜨지 않는 것을 확인할 수 있습니다.

<p align="center">
  <img src="/assets/images/scrollview/9.png" width="520px">
</p>



이제 여기에 `UIStackView`를 하나 추가하여 `contentView`에 꽉 차도록 오토레이아웃을 준 후에 `@IBOutlet`으로 stackView를 프로퍼티로 만들어서 아래와 같이 코드를 작성합니다.

아래 코드는 랜덤 색상의 100에서 400 높이를 가진 10개의 뷰를 생성하여 stackView에 넣어주는 코드입니다.

```swift
import UIKit

class VerticalScrollViewController: UIViewController {

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



#### 구현 화면

<p align="center">
  <img src="/assets/images/scrollview/vertical.gif" width="320px">
</p>



<br>

### 마치며



<br>

#### References

- 