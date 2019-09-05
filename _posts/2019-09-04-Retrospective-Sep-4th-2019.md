---
title: "소개딩 해커톤 회고"
header:
  teaser: /assets/images/retrospective.jpg
  overlay_image: /assets/images/retrospective.jpg
  overlay_filter: 0.2
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - 회고
tags:
  - iOS
  - Swift
  - 해커톤
  - Hackathon
---



### 소개딩이란?

행정안전부가 주최하고 한국인터넷진흥원이 주관하는 ‘2019 소프트웨어(SW) 개발보안 경진대회

2019 소프트웨어(SW) 개발보안 경진대회는 Java 또는 Python 언어를 선택하여 주제에 맞는 안전한 소프트웨어를 개발 하는 해커톤으로 국내 소재 대학에 재학·휴학 중인 대학생·대학원생으로 구성된 팀(4인 이내)단위로 지원할 수 있습니다.

[소개딩 홈페이지](http://securecoding.software/main.php)

<img src="https://cfile1.onoffmix.com/attach/CeY1x4l2J6ZVOAhsmL8QHTE9cpoy3Nfk" width="500" >

<br/>

### 해커톤 진행방식 및 후기

> 해커톤까지

서류심사와 사전설명회 그리고 사전교육을 필수로 참여해야했고 본선 대회는 8월 22일-23일 무박 2일로 진행되었다.
지방에 거주하여 서울까지 필수로 참여해야해서 번거로움이 있었다. 교통비 지원은 없어서 아쉬웠다.



> 해커톤 당일

'소개딩'이란 이름부터 개인적으로 올드한 느낌을 많이 받았다. 티셔츠 디자인이나 색깔도 촌스러웠다. KISA에서 진행하다 보니 상당히  돈이 많은 것 같았다. 기본적으로 주는 키트도 짱짱했고 상당히 좋은 호텔에서 진행되었고 호텔 직원분들이 직접 식사를 가져다 주셨다. (이 돈 아껴서 다른데 쓰지...)

해커톤 진행이나 사용된 문구들이 상당히 올드했다. 해커톤 개발 시간을 **매력발산시간**이라고 한게 충격적이었다. 하지만 staff들은 정말 친절하시고 좋았다.



> 심사 평가


심사는 크게 두개로 나뉘어져 **개발 부문**, **보안 부문**을 기준으로 평가되었다.

**MS Azure 클라우드 서비스** 및 **스패로우 진단도구** 사용 여부 등을 통해 가산점이 주어졌다.



### 아쉬웠던 점

심사위원

평가 요소 (UI)

가산점



### 좋았던 점

철저하게 분리된 프론트와 백엔드



### 개발 진행 중, 문제점

>I hate you, Dynamic

collectionView에서 Dynamic하게 containerView의 크기를 조절하고 싶었지만, 맘처럼 되지 않았다. 이전에 들었던 강의에서 사용한 방법을 적용했지만 UITextView에 한하여 적용되는 코드였다.

아무리 찾아봐도 해결책을 찾지 못하여 결국...

UITextView를 기반으로 크기를 얼추 계산하여 맞추었다. 그래도 이게 잘 통하여서 다행이었다.

```swift
if let contentsDicts = item.resultDataDictionary["contents"] as? [Dictionary<String, AnyObject>] {
                var contentsList: String = ""
                contentsDicts.forEach { (dict) in
                    let twoLinesText = "HELLO\nGOODBYE"
                    contentsList += twoLinesText + "\n"
                }
                textView.text = contentsList
  
  							...
            }
```



그리고 재밌는 사실을 하나 알게되었는데, collectionView는 스크롤 할 때마다 새롭게 로딩하기 때문에 꼭 **removeFromSuperview()**를 컨텐츠들을 subview로 추가하기 전에 해주어야한다.

```swift
                stackView.arrangedSubviews.forEach({ $0.removeFromSuperview() })
                contentsDicts.forEach { (contentDict) in
                    let customContentView = CustomContentView(contentDict: contentDict)
                    customContentView.contentTextView.text = contentDict["content"] as! String
                    stackView.addArrangedSubview(customContentView)
                }
```

그렇지 않으면 스크롤 할 때마다 계속해서 contents가 추가되는 끔찍한 현상이 발생한다.



> Darn it, WebKit

믿었던 WebKit의 배신, 결국 시연할 때 배신했다.



> Hurry up! bro!

마지막에 급하게 만들어서 일까, 마음대로 컨텐츠들의 크기나 비율을 조절하기 어려웠다.

stackView에 넣으면 모두 해결될 줄 알았는데, 흔히 마법의 stackView라고 할 정도로 stackView를 애용하시는 분들 처럼 잘 사용하려면 stackView에 대해 공부를 더 해봐야겠다.