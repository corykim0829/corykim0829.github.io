---
title: "[iOS] UICollectionViewCell Dynamic Height, 동적 높이 구현하기 with Dummy Cell"
header:
  teaser: /assets/images/ios-posts2.jpeg
  overlay_image: /assets/images/ios-posts2.jpeg
  overlay_filter: 0.2
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - iOS
tags:
  - UICollectionView
  - UICollectionViewCell
  - UICollectionViewCell Dynamic Height
  - Dummy cell
date: 2020-07-03 16:25:00
lastmod: 2020-07-03 16:25:00
sitemap :
  changefreq : daily
  priority : 1.0
---

Dummy cell을 이용하여, UICollectionViewCell의 동적 높이 구해보자.

<br>

### 들어가며

Github 모바일앱과 같은 이슈 관리 앱 개발 프로젝트를 진행하면서 이슈목록을 UICollectionView로 구현하면서, 이슈 데이이터에 따라 **동적 높이**를 가진 `UICollectionViewCell`을 구현하고 싶었습니다. 컬렉션뷰 셀의 동적 높이를 구현하는 것은 dynamic height라는 키워드로 검색해보면 쉽게 찾아서 구현할 수 있습니다. 하지만 대부분의 예제는 텍스트나 이미지 등과 같은 컨텐츠의 높이를 계산해서 적용해주는 것이었습니다. 

저는 컬렉션뷰 셀이 하위 객체로 **동적으로 높이가 변할 수 있는 UICollectionView**를 가지는 구조를 구현하고 싶었습니다. 저는 **Dummy cell을 사용한 사이즈 계산법**을 사용했습니다. 주변의 좋은 조언과 삽질 끝에 완성을 했고 이를 어떻게 구현했는지 공유해보려고 합니다. 😀

<br>

### 구현 화면

<p align="center">
  <img src="/assets/images/collectionview-cell-dynamic-height/final2.gif" width="320px">
</p>


Github처럼 이슈에 **레이블**을 붙일 수 있는 기능이 있으며, 이슈에 레이블이 많아져 레이블이 차지하는 공간이 많아지면 이에 따라 동적으로 셀 높이를 결정하도록 구현하였습니다.

<br>

### IssueCell 레이아웃 구성

`IssueCell`은 `IssuesCollectionView`의 셀로 등록되어있으며 구성은 아래와 같습니다.

<p align="center">
  <img src="https://github.com/corykim0829/corykim0829.github.io/blob/master/assets/images/collectionview-cell-dynamic-height/whole-layout.png?raw=true" width="740px">
</p>


#### ContentsStackView

`IssueCell`의 주요 subview로는 커스텀 `UIStackView`인 `ContentsStackView`가 있으며, 이슈에 관한 정보들을 나타내는 뷰들을 담고 있습니다. 가장 상단에 이슈의 제목을 나타내는 `TitleLabel`과 바로 아래 작성자 정보를 나타내는 `WriterLabel`은 커스텀 `UILabel`로 `IssueCell`에서 항상 default로 존재하는 뷰 요소들입니다. (이 외의 뷰 요소들은 dynamic height를 구현하는데 있어서 큰 영향이 없기 때문에 설명하지 않겠습니다.)

#### IssueLabelsViewController

레이블을 나타내는 클래스는 `IssueLabelsViewController`로 `UICollectionView`가 아닌 `UIViewController`로 구현하였는데, 그 이유는 `UICollectionView`로 구현하게 되면, 레이블을 표시하기 위해서 `UICollectionViewDataSource`, `UICollectionViewDelegateFlowLayout`와 같은 프로토콜을 채택하여 데이터를 주입하는 것과 같은 **동작**을 구현해야 합니다. 뷰는 재사용을 고려하여 UI를 구성하고 데이터를 표시해주는 일만 처리하기 위해서 `IssueLabelsViewController`라는 컨트롤러가 일을 처리하도록 구현하였습니다.

`IssueLabelsViewController`를 `UIStackView`에 arrangedSubview로 추가하기 위해서는 `issueLabelsViewController.view`를 `addArrangedSubview()` 메소드를 통해 추가하면 됩니다.

<br>

#### IssueCell layout

<p align="center">
  <img src="/assets/images/collectionview-cell-dynamic-height/layout-hierarchy.png" width="440px">
</p>


이후에 더 자세하게 설명드리겠지만, 동적 높이를 조금 더 편하게 구현하기 위해서 **UI**와 **Layout**을 모두 **코드로 구현**하였습니다. `ContentsStackView`가 `IssueCell`의 높이를 결정하는 것에 영향을 가장 많이 주기 때문에 `ContentsStackView`과 `IssueCell`에 간 오토레이아웃을 꼭 주어야합니다.

`IssueCell`이라는 `UICollectionViewCell`을 상속받아서 만든 클래스를 코드로 구현했습니다.

<br>

### IssueCell 사이즈 결정 시점

동적 높이를 구현하기 위해서는 UICollectionviewCell의 사이즈가 결정되는 시점을 알아두면 좋습니다. 간단하게 아래와 같습니다.

<p align="center">
  <img src="/assets/images/collectionview-cell-dynamic-height/item-size.png" width="820px">
</p>

- 첫번째로,  `UICollectionViewDataSource`의 `collectionView(_:numberOfItemsInSection:)` 메소드를 통해 먼저 몇개의 셀이 있는지 확인합니다. 
- 다음으로, **item의 개수가 0개가 아니라면** `UICollectionView`는 셀의 사이즈를 알기 위해서 `UICollectionViewDelegateFlowLayout`를 채택한 객체에 구현된 `collectionView(_:layout:sizeForItemAt:)` 메소드에서 반환한 사이즈를 통해 셀의 사이즈를 결정합니다.
- 마지막으로, DataSource는 `collectionView(_:cellForItemAt:)` 메소드를 통해 indexPath에 맞는 cell을 반환합니다.

<br>

### Dummy cell을 통한 동적 높이 구하기

`UICollectionViewCell`의 동적 높이를 구할 수 있는 방법 중 하나로, **실제로는 사용하지 않을 dummy cell**의 객체를 만들어서 **size**만 가져와서 사용하는 것입니다. Dummy cell의 객체를 직접 만들어서 사용해야하는 부분 때문에 `IssueCell`의 UI를 모두 코드로 구현하였습니다. 

<br>

#### Dummy cell 만들기

먼저 dummy cell을 만들기 위해 `IssueCell` 객체를 생성해줍니다. 여기서 `IssueCell`을 생성할 때, `CGRect` 타입을 파라미터로 받는 **생성자**를 사용하여 생성하려는 셀의 높이는 늘어날 컨텐츠의 사이즈를 고려하여 **넉넉하게** 잡아주면 됩니다. 

예제로는 제가 만들고 싶은 `IssueCell`의 width는 `IssuesCollectionView` 상위 뷰컨트롤러의 width의 0.9로 고정되어있으며, 높이는 300이 넘지 않을 것이기 때문에 `estimatedHeight`에 300 값을 저장하여 dummy cell의 사이즈를 설정해주었습니다. 

```swift
func collectionView(
        _ collectionView: UICollectionView,
        layout collectionViewLayout: UICollectionViewLayout,
        sizeForItemAt indexPath: IndexPath) -> CGSize {
  
        let width = view.frame.width * 0.9
        let estimatedHeight: CGFloat = 300.0
        let dummyCell = IssueHorizontalCell(
            frame: CGRect(x: 0, y: 0, width: width, height: estimatedHeight))
```

<br>

#### Dummy cell에 실제 데이터 넣기

이제 실제 cell보다 넉넉하게 만들어진 dummy cell에 **실제 데이터**를 넣어줘야합니다. 현재 `IssuesCollectionView`의 dataSource인 `IssuesCollectionViewDataSource`는 분리된 클래스로 **뷰모델** 역할을 하기 때문에 서버로부터 이슈 정보를 가져오면 `IssuesCollectionView`를 reload 하도록 바인딩되어있습니다. 


이슈 데이터는 모두 **dataSource**가 가지고 있기 때문에 dataSource에 있는 실제 데이터를 가져오기 위해서 `referIssue(at:handler:)` 메소드를 사용합니다. handler라는 클로저 매개변수를 사용하여 dataSource의 특정 indexPath의 이슈 데이터를 사용할 수 있습니다.

```swift
func referIssue(at indexPath: IndexPath, handler: (Issue) -> Void) {
    let issue = issues[indexPath.item]
    handler(issue)
}
```

<br>

DataSource를 통해 데이터를 사용하는 구조는 다음과 같습니다.

<p align="center">
  <img src="/assets/images/collectionview-cell-dynamic-height/vc-datasource-structure.png" width="800px">
</p>

가져온 이슈 데이터로 dummy cell을 구성해주기 위해 `IssueCell`의 `configureCell(with:)` 메소드를 호출합니다. 실제 정보를 기반으로 셀을 구성해주는 메소드입니다. `ContentsStackView`에 있는 Title, Writer를 업데이트하고, 가장 중요한 **레이블**들을 업데이트해주는 작업들을 실행하게 됩니다.

<br>

#### 내부 IssueLabelsCollectionView height 잡기

Dummy cell을 실제 데이터로 업데이트 해줄 때에 가장 중요한 부분은 **유동적으로 변하는 개수를 가진 레이블들을 담는** `IssueLabelsCollectionView`입니다. 레이블을 몇개를 가지고 있느냐에 따라서 높이가 변경되기 때문입니다.

`IssueLabelsCollectionView`는 `IssueLabelsViewController`의 하위 객체로 전달받은 이슈 데이터를 기반으로 업데이트하여 **정확한 높이**를 결정해야합니다.

따라서 `IssueCell` 의 `configureCell(with:)`에서 아래와 같이 두개의 메소드 호출을 통해 **이슈 데이터의 레이블 정보**를 전달해주고 `IssueLabelsCollectionView`를 `reload` 해줍니다.

```swift
issueLabelsViewController.updateLabels(issue.labels)
issueLabelsViewController.reloadCollectionView()
```

<br>

위 과정을 마쳤다면 `IssueLabelsViewController`의 `IssueLabelsCollectionView`는 indexPath에 맞는 라벨들의 정보를 정상적으로 표시합니다. 이제 이 뷰컨트롤러를 `ContentsStackView`에 추가해줍니다.

```swift
contentsStackView.addArrangedSubview(issueLabelsViewController.view)
```

<br>

`UIStackView`에 추가되면 자동적으로 `UIStackView` 내부에서 auto layout이 적용되는데, 여기서 `IssueLabelsCollectionView`의 크기만큼 `UIStackView`에서 자리를 잡을 수 있도록 아래와 같이 `issueLabelsViewController.view`의 heightAnchor를 명시해줍니다. 

```swift
layoutIfNeeded()
issueLabelsHeightConstraint = issueLabelsViewController.view.heightAnchor.constraint(
    equalToConstant: issueLabelsViewController.contentHeight)
issueLabelsHeightConstraint.isActive = true
```

`issueLabelsViewController.contentHeight`는 computed property로 `IssueLabelsCollectionView`의 contentSize.height를 반환합니다.
{: .notice--warning}

`issueLabelsHeightConstraint`라는 변수로 따로 프로퍼티로 사용하는 이유는 IssueCell이 **재사용**될 때 heightAnchor constraint를 중복으로 지정해주기 때문에 이를 방지하기 위해 사용하였습니다.
{: .notice--danger}

<br>

위와 같은 작업은 이슈 데이터의 레이블이 있을 때에만 적용되어야 합니다. 그렇기 때문에 guard 문으로 레이블이 있을 때에만 실행될 수 있도록 처리해주었습니다. 

이슈의 레이블 뷰컨트롤러의 사이즈를 결정하는 전체 코드를 보면 아래와 같습니다.

```swift
private func configureIssueLabels(with issue: Issue) {
    guard issue.labels.count > 0 else { return }
    issueLabelsViewController.updateLabels(issue.labels)
    issueLabelsViewController.reloadCollectionView()
    contentsStackView.addArrangedSubview(issueLabelsViewController.view)
    layoutIfNeeded()
    issueLabelsHeightConstraint = issueLabelsViewController.view.heightAnchor.constraint(
        equalToConstant: issueLabelsViewController.contentHeight)
    issueLabelsHeightConstraint.isActive = true
}
```

<br>

#### EstimatedSize 만들기

이제 dummyCell의 **내부 컨텐츠의 사이즈의 정확한 값**도 정해졌기 때문에 실제로 반환할 **cell의 사이즈**를 결정해야합니다.

실제 이슈 데이터를 통해 업데이트된 dummy cell을 `layoutIfNeeded()`와 `systemLayoutSizeFitting(_:)` 메소드를 사용하여 **예상 사이즈**를 잡아줄 것입니다.

`systemLayoutSizeFitting(_:)`는 **view의 현재 constraints를 기반으로 최적의 사이즈를 반환하는 메소드**입니다. 그렇기 때문에 넉넉하게 생성되었던 dummy cell을 **현재 constraints에 가장 딱 맞는 사이즈**로 반환해줍니다. 파라미터에는 뷰에 희망하는 사이즈를 넣게 되어있습니다. 처음 dummy cell을 만들 때 설정해주었던 사이즈를 사용하면 됩니다. 반환된 `CGSize`값은 `estimatedSize` 지역변수로 저장합니다.

```swift
dummyCell.layoutIfNeeded()
let estimatedSize = dummyCell.systemLayoutSizeFitting(
    CGSize(width: width, height: estimatedHeight))
```

<br>

`collectionView(_:layout:sizeForItemAt:)`의 전체 코드는 아래와 같습니다.

```swift
func collectionView(
        _ collectionView: UICollectionView,
        layout collectionViewLayout: UICollectionViewLayout,
        sizeForItemAt indexPath: IndexPath) -> CGSize {
  
        let width = view.frame.width * 0.9
        let estimatedHeight: CGFloat = 300.0
        let dummyCell = IssueCell(
            frame: CGRect(x: 0, y: 0, width: width, height: estimatedHeight))
        dataSource.referIssue(at: indexPath) { (issue) in
            dummyCell.configureCell(with: issue)
        }
        dummyCell.layoutIfNeeded()
        let estimatedSize = dummyCell.systemLayoutSizeFitting(
            CGSize(width: width, height: estimatedHeight))
        return CGSize(width: width, height: estimatedSize.height)
    }
```

<br>

### 마치며

`UICollectionViewCell`의 동적 높이를 구현해보았습니다. 사실 `UICollectionViewLayout`을 사용해서 구현해보고 싶었는데, 아직은 조금 더 공부를 해봐야할 것 같습니다. 😅 

`IssueLabelsCollectionView`의 label cell도 텍스트 길이에 따라 동적으로 width가 변동되는데, 이 내용까지 넣으면 너무 길어질 것 같아서 넣지 않았습니다. 추후에 시간이 되면 추가 작성하여 링크를 걸 예정입니다. 피드백은 언제나 환영합니다! 긴 글 읽어주셔서 감사합니다. 👦🏻

<br>

#### 도움 주신 감사한 분들

- Mason
- Jinie
- Delma
- Gangwoon

<br>

#### References

- [systemLayoutSizeFitting(_:)](https://developer.apple.com/documentation/uikit/uiview/1622624-systemlayoutsizefitting)
- [layoutIfNeeded()](https://developer.apple.com/documentation/uikit/uiview/1622507-layoutifneeded)