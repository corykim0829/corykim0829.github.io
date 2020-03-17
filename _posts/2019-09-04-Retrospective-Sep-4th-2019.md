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
layout: post
date: 2019-09-04 12:00:00
lastmod: 2019-09-04 12:00:00
sitemap :
  changefreq : daily
  priority : 1.0
---

한국인터넷진흥원 주관 2019 SW 개발보안 경진대회 '소개딩'을 마치며 회고를 작성해 본다.

<br>

### 소개딩이란?

행정안전부가 주최하고 한국인터넷진흥원이 주관하는 ‘2019 소프트웨어(SW) 개발보안 경진대회'이다.

2019 소프트웨어(SW) 개발보안 경진대회는 Java 또는 Python 언어를 선택하여 주제에 맞는 안전한 소프트웨어를 개발 하는 해커톤으로 국내 소재 대학에 재학·휴학 중인 대학생·대학원생으로 구성된 팀(4인 이내)단위로 지원할 수 있다.

<br>

<p align="center">
  <a href="http://securecoding.software/main.php">소개딩 홈페이지</a>
</p>

<p>
  <img src="https://cfile1.onoffmix.com/attach/CeY1x4l2J6ZVOAhsmL8QHTE9cpoy3Nfk" width="500" >
</p>
<br/>

### 해커톤 진행방식

> 해커톤까지

서류심사와 사전설명회 그리고 사전교육을 필수로 참여해야했고 본선 대회는 8월 22일-23일 무박 2일로 진행되었다.
지방에 거주하여 서울까지 필수로 참여해야해서 번거로움이 있었다. 교통비 지원은 없어서 아쉬웠다.

<br>

> 해커톤 당일

'소개딩'이란 이름부터 개인적으로 올드한 느낌을 많이 받았다. 티셔츠 디자인이나 색깔도 촌스러웠다. KISA에서 진행하다 보니 상당히  돈이 많은 것 같았다. 기본적으로 주는 키트도 짱짱했고 상당히 좋은 호텔에서 진행되었고 호텔 직원분들이 직접 식사를 가져다 주셨다. (이 돈 아껴서 다른데 쓰지...)

해커톤 진행이나 사용된 문구들이 상당히 올드했다. 해커톤 개발 시간을 **매력발산시간**이라고 한게 충격적이었다. 이 밖에도 몇개 더 있었다... 하지만 staff들은 정말 친절하시고 좋았다.

<br>

> 심사 평가


심사는 크게 두개로 나뉘어져 **개발 부분(60)**, **보안 부분(40)**을 기준으로 평가되었다. 

**MS Azure 클라우드 서비스** 및 **스패로우 진단도구** 사용 여부 등을 통해 5개 항목에서 **가산점(10)**이 주어졌다.

해커톤인 만큼 개발 부분에 비중이 큰 것 같았다. 

<br>

### HACK

> Beginning

팀원은 4명으로 3명은 백엔드 그리고 iOS 나 혼자였다. 이미 기획은 마쳐졌고 나는 기획에 맞추어 개발만 진행하면 되었다. 기획에 참여하지 않았기 때문에, 프로젝트에 대한 이해도가 다른 팀원들에 비해 떨어졌다.

<br>

> Developing

소개딩에서 사용하기 권장했었던 MS Azure 클라우드 서비스를 통해 서버를 구축했고, 나는 그 서버와 HTTP 통신을 하며 데이터를 주고 받았다. 

프로젝트 진행에 있어서는 매우 수월했다. 백엔드와 프론트가 분리되니 나는 철저히 iOS 개발에 집중할 수 있어서 좋았다.

백엔드와 함께 어떤 식으로 json을 넘겨주면 좋을 것인지 생각하며 이야기 나누는 것도 좋았다. 

<br>

### 아쉬웠던 점

> 심사 기준과 심사위원

가장 아쉬웠던 게 이 부분이 아닐까 싶다. 이전에 언급한 심사 기준을 보게 되면 **개발 부분**이 많은 부분을 차지하는 것 같았고, 모든 팀의 작성한 코드량이 많지 않기 때문에, 보안 부분에는 큰 차이가 없을 것 같았다. 

하지만 결과는 조금 의아했다. 참가팀 상호평가를 위해 돌아다니면서 다른 팀들의 작품들을 보았기에 몇가지 작품은 정말 깔끔한 UI로 완성도가 높았고, 어떤 작품은 UI에 투자를 정말 안한 것이 느껴지는 작품들이 있었다. 그런데 내 생각과는 달리 디자인에 신경쓰지 않은 팀이 수상을 하고 신경쓴 팀은 수상도 하지 못하였다. 이렇게 까지 말하는 이유는 개발부분에서 완성도라고 하여 **구현의 우수성**과 **디자인의 우수성**이 무려 **30점**을 차지하고 있었기 때문이다. 명시된 심사기준이 정확하게 평가되었는지 의문이 들었다. 하고싶은 말이 더 있지만, 할많하않...

<br>

> 가산점

5개의 가산점 항목이 있었는데, 그 중 Azure 서비스가 크레딧 문제로 인하여 모든 팀이 가산점을 얻게 되었다. 백엔드 팀원들이 고생해서 Azure 서비스를 적용했는데, 갑자기 해커톤 당일에 가산점이 날아가 버리니 허탈했다.

<br>

### 좋았던 점

> 사육 in 해커톤

좋았던 점은 식사였다. 가만히 앉아있으면 호텔 직원분들이 식사를 가져다 주셨다. 밥도 맛있었고 간식도 정말 많이 줬다. 그래서 사육당하는 기분이었다.

<br>

> 쉬는 공간

해커톤 공간 옆에는 쉴수 있게 에어쇼파와 에어침대가 있어 중간중간 잠을 잘 수 있어서 좋았고, VR도 준비되어 있어서 리프레쉬할 수 있어서 좋았다.

<br>

### 개발 진행 중, 문제점

>너무 어려운 Dynamic Cell

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

<br>

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

<br>

collectionView나 tableView는 정말 자주 쓰이기 때문에 더 공부해야할 것 같다는 생각이 들었다.

<br>

> WebKit

믿었던 WebKit의 배신... 그저 url을 띄우는 webView를 만들었는데, 중간중간 버벅거리면서 웹페이지가 뜨지 않았다. 근데 이게 웃긴데 어느 페이지는 잘 뜨고 어느 페이지는 잘 뜨지 않아서 너무 짜증이 났다. 결국 해결하지는 못했고 시연할 때 버벅거려서 너무 아쉬웠다.

<br>

> 간단한 UI 작업

마지막에 급하게 만들어서 일까, 마음대로 컨텐츠들의 크기나 비율을 조절하기 어려웠다.

stackView에 넣으면 모두 해결될 줄 알았는데, 흔히 마법의 stackView라고 할 정도로 stackView를 애용하시는 분들 처럼 잘 사용하려면 **stackView**에 대해 공부를 더 해봐야겠다.

<br>

### 마치며

내년에도 소개딩을 개최할 것 같은데, 내년에는 개발 부분을 평가를 더 제대로 해주면 좋겠다. 해커톤으로 바뀐 뒤 1회여서 그런지 아쉬운 부분이 많았다.

그래도 좋은 기회로 소개딩 해커톤을 하게 되어서 굉장히 영광이었고, 단시간에 개발을 해보니 내가 어느 부분이 부족한지 알 수 있었다. 또한 계속 써봐야겠다고 생각만 한 `Alamofire` 라이브러리를 통해 HTTP 통신을 해볼 수 있어서 좋았다. 

해커톤이 끝나고는 힘이 들어서 개발을 조금 쉬었지만 회고 후에 다시 열심히 개발해보려고 한다.