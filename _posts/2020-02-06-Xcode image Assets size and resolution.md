---
title: "Xcode Assets image size and resolution"
header:
  teaser: /assets/images/ios-posts2.jpeg
  overlay_image: /assets/images/ios-posts2.jpeg
  overlay_filter: 0.2
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - iOS
tags:
  - iOS
  - H.I.G
---

Xcode 프로젝트 Assets에 넣은 이미지 사이즈와 해상도 설정

<br>

배경을 패턴으로 지정하려고 하는데, 해상도 문제로 고민하다 정리하게 되었다. 

### Image Size and Resolution in HIG - 번역

iOS가 화면에 UIView 클래스와 같은 컨텐츠를 배치하는 데 사용하는 좌표 시스템은 **포인트 단위의 측정을 기반**으로 하며, 이는 디스플레이의 **픽셀**에 매핑된다. 표준 해상도 디스플레이는 1:1 pixel 밀도 (`@1x`), 하나의 픽셀이 하나의 포인트와 대응된다는 말이다. 고화질 디스플레이는 더 높은 pixel 밀도를 가지는데, 배율을 2.0 또는 3.0(`@2x`, `@3x`)을 제공한다. 결과적으로 고화질 디스플레이는 더 많은 픽셀을 가진 이미지를 요구한다.

<p align="center">
	<img src="https://developer.apple.com/design/human-interface-guidelines/ios/images/ImageResolution-Graphic_2x.png" width="540px">
</p>

예를 들어, `100px x 100px`의 표준 화질 이미지가 있다고 하자. `@2x` 버전의 이미지는 `200px x 200px`이고 `@3x` 버전의 이미지는 `300px x 300px` 이여야한다.

앱에 있는 모든 artwork에 앱이 지원하는 모든 디바이스의 고화질 이미지를 제공해야한다. 디바이스에 따라 각 이미지의 픽셀 수에 특정 배율을 곱하면 된다.

<br>

- iOS의 좌표 시스템 포인트 단위 기반
- 디스플레이의 픽셀에 매핑해야한다.
- 매핑 scale은 `@1x`, `@2x`, `@3x` 가 있다.
- 기기마다 지원하는 scale이 있다.

---

### Upsacling & Downscaling

명확하게 크기를 지정하지 않으면 아래 두가지 일이 일어날 수 있다.

#### Upscaling

`@3x`와 `@2x` 이미지는 `@1x` 이미지에서 확대대여서 나타나질 수 있다. 하지만 이러한 경우 이미지가 흐릿해져 보기에 좋지 않다. `@2x`에서 확대된 `@3x`는 subpixel들을 사용해야하여 더욱 좋지 않는 결과가 나올 수 있다.

#### Downscaling

보통 upscaling 결과보다는 좋은 편이지만, 모든 이미지가 그런 것은 아니다. `1px` 테두리가 있는 `@3x` 이미지가 `@1x` 이미지로 downscaling되면 테두리가 보이지 않는다. (0.33px). 이미지 안에 다른 작은 객체들에 똑같이 적용된다. Downscaling은 세부사항을 없애버린다.

이러한 upsacling과 downscaling 현상없이 깔끔하게 이미지를 사용하고 싶다면, 항상 `@2x` 와 `@3x` 이미지를 꼭 사용해야한다. 문제점이 발견되면 `@1x` 이미지를 추가하면 된다. 고화질의 이미지는 upscaling을 해결할 수는 있지만 downscaling은 해결할 수 없다. Downscaling이 일어날 때 scale의 gap이 클수록 더 좋지 않은 결과를 얻는다.

애플에는 3가지 종류의 디바이스가 있어 이미지를 Xcode Assets에 추가할 때 3가지 종류의 이미지가 필요하다.

- `1 pixel = 1 point` : `@1x` (오래된 아이폰, 아이패드)
- `4 pixels (2 x 2) = 1 point` : `@2x`
  - iPhone SE, iPhone 6s, 7, 8, XR, 7.9" iPad mini 4, ...
- `9 pixels (3 x 3) = 1 point` : `@3x`
  - iPhone 6s Plus, iPhone 7 Plus, iPhone 8 Plus, iPhone X, iPhone Xs, iPhone Xs Max, ...

실 사용되는 기기들은 거의 `@2x` 이상이기 때문에 `@1x` 이미지는 고려할 필요는 없다.

<p align="center">
	<img src="https://i.stack.imgur.com/QcuUy.jpg" width = "400px">
</p>

---

HIG에 명시된 iOS에서의 Image 크기와 해상도에 대해서 알아본 뒤 실제로 패턴이미지를 사용한 배경색깔 지정을 시도해 보았다.

### UIColor

> init(patternImage:)

```swift
let patternColor = UIColor(patternImage: #UIImage)
view.backgroundColor = patternColor
```

위와 같이 설정하면 패턴으로 설정이된다. 사용되는 UIImage의 사이즈나 해상도에 관한 언급이 문서에 없어 패턴 크기를 더 작게 만들 수는 없을까를 고민했다.

UIImage를 패턴으로 만들기 때문에 UIImage 자체 사이즈를 줄이면 될 것 같다 싶어, UIImage 사이즈를 조절하는 코드를 찾아보았고, CGContext를 사용하여 UIImage 사이즈를 조절해보았다.

```swift
extension UIImage {
    func resize(scale: CGFloat) -> UIImage? {
        let transform = CGAffineTransform(scaleX: scale, y: scale)
        let size = self.size.applying(transform)
        UIGraphicsBeginImageContext(size)
        self.draw(in: CGRect(origin: .zero, size: size))
        let resultImage = UIGraphicsGetImageFromCurrentImageContext()
        UIGraphicsEndImageContext()
        return resultImage
    }
}
```

원하던 더 자잘한 패턴을 결과로 얻었지만 테두리가 조금 여백이 생긴 것처럼 어색해졌다.

#### Assets.xcassets scale 설정

Assets에 기본적으로 추가하면 이미지의 scale 설정은 `@1x`로 올라가 있어 이를 변경해 보았다. 

원본 이미지 사이즈:  `72px x 72px`

<p align="center">
	<img src="/assets/images/image-size-res/image-size-res1.jpeg" width = "800px">
	<img src="/assets/images/image-size-res/image-size-res2.jpeg" width = "800px">
</p>

결과는 성공적이었다. `@2x` 기기와 `@3x` 기기 간의 큰 차이는 확인할 수는 없었다.  기본적으로 설정이 `@1x`로 되어있어 `@2x`

, `@3x` 지원 기기로 upscaling 되는 거는 이해가 되는데, 두 기기간 차이가 없으니 이해가 되지를 않는다.

결론은 패턴 이미지 사이즈 조절은 이미지 사이즈 자체를 조절하는게 편하다.

<br>

#### References

- [Apple Human Interface Guidelines - Image Size and Resolution](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/image-size-and-resolution/)
- [Stackoverflow - Why do I need @1x, @2x and @3x iOS images?](https://stackoverflow.com/questions/32354453/why-do-i-need-1x-2x-and-3x-ios-images)
- [UIColor initializer : patternImage](https://developer.apple.com/documentation/uikit/uicolor/1621933-init)
- [UIImage resizing](https://soooprmx.com/archives/9670)