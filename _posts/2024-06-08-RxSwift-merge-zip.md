---
title: "[RxSwift] Merge와 Zip의 차이점"
header:
  teaser: /assets/images/rxswift-logo.jpg
  overlay_image: /assets/images/rxswift-logo-horizon.png
  overlay_filter: 0.1
categories:
  - RxSwift
tags:
  - RxSwift
  - RxSwift Merge
  - RxSwift Zip
date: 2024-06-08 11:21:00
lastmod: 2024-06-08 11:21:00
sitemap :
  changefreq : daily
  priority : 1.0
---

RxSwift의 연산자 Merge와 Zip에 대해서 알아보자

<br>

RxSwift 연산자 Merge와 Zip은 자주 사용되는 연산자들 중에 하나입니다. 여러 Observable들을 하나의 Observable로 바꿔주는 역할을 합니다. 두 연산자의 특징과 차이점을 알아보도록 하겠습니다.

<br>

#### Merge

Merge는 여러 Observable에서 방출하는 값들을 하나의 Observable로 합쳐주는 연산자입니다.

<p align="center">
  <img src="/assets/images/rxSwift/operator-merge.png" width="600px">
</p>

```swift
let disposeBag = DisposeBag()
let observable1 = PublishSubject<String>()
let observable2 = PublishSubject<String>()
let mergedObservable = Observable.merge(observable1, observable2)

mergedObservable
    .subscribe(onNext: { value in
        print(value)
    })
    .disposed(by: disposeBag)

observable1.onNext("1")
observable2.onNext("2")
observable1.onNext("3")
observable2.onNext("4")
observable2.onNext("5")

// 결과
// 1
// 2
// 3
// 4
// 5
```

두개의 `PublishSubject`를 만들어서 `Observable.merge` 메소드를 이용하여 두 `Observable`을 합칠 수 있습니다. 

```swift
let mergedObservable = Observable.merge(observable1, observable2)
```

Merge로 인하여 합쳐진 Observable `mergedObservable`을 구독하여 방출되는 값을 확인해봅니다. `observable1`과 `observable2`에 `onNext()` 메소드를 사용하여 값을 보내게 되면 합쳐진 Observable`mergedObservable`에서 모든 값을 잘 받아서 처리하는 것을 확인할 수 있습니다.

```swift
let observables = [observable1, observable2]
let mergedObservable = Observable.merge(observables)
```

`Observable.Merge`는 파라미터로 컬렉션을 받는 것도 지원하고 있습니다.



또한 Merge를 사용할 때에는 같은 타입의 Element를 사용하는 Observable들만 합칠 수 있습니다.

```swift
let observable1 = PublishSubject<String>()
let observable2 = PublishSubject<String>()
let mergedObservable1 = Observable.merge(observable1, observable2)

let observable3 = PublishSubject<Int>()
let observable4 = PublishSubject<String>()
let mergedObservable2 = Observable.merge(observable3, observable4) // complie error
```



<br>

####Zip

<p align="center">
  <img src="/assets/images/rxSwift/operator-zip.png" width="600px">
</p>

Zip은 Merge 연산자보다 조금 더 설명이 많은걸 확인할 수 있습니다. Merge와 마찬가지로 여러개의 observable을 하나의 observable로 합치지만, **combination**으로 넘어온다는 특징이 있습니다.

마블 다이어그램에서도 merge와 다르게, 각 observable의 값이 합쳐져서 나오는 것을 확인할 수 있습니다.

```swift
let disposeBag = DisposeBag()
let observable1 = PublishSubject<Int>()
let observable2 = PublishSubject<String>()
let zippedObservable = Observable.zip(observable1, observable2)

zippedObservable
    .subscribe(onNext: { value in
        print(value)
    })
    .disposed(by: disposeBag)

publishSubject1.onNext(1)
publishSubject2.onNext("A") // (1, "A")
publishSubject1.onNext(2)
publishSubject2.onNext("B") // (2, "B")
publishSubject1.onNext(3)
```

Zip에서는 값이 **튜플**로 나오는 것을 확인할 수 있고, 합쳐진 observable이 **combination**으로 짝지어질 때에만 값이 나오는 걸 확인할 수 있습니다. 하나의 observable만 값을 방출할 때에는 처리가 되지 않는 것을 확인할 수 있습니다.



RxSwift에서 zip 메소드의 설명을 보면 문서에서 말한 **combination**에 대해 조금 더 이해할 수 있습니다.

**zip(_:_:)**
Merges the specified observable sequences into one observable sequence of **tuples** whenever all of the observable sequences have produced an element at a corresponding index.
{: .notice--warning}

일치하는 index에서 생성된 값들이 튜플로 넘어오게 구현되어있다는 것을 확인할 수 있습니다.



또한 Merge와 다르게 다른 타입의 Element의 Observable을 합치는 것이 가능합니다.



zip도 마찬가지로 컬렉션을 파라미터로 받아서 합칠 수 있습니다. 여기서 재밌는 점은 컬렉션으로 받게되면, 튜플로 값이 처리되지 않고 컬렉션으로 값이 처리된다는 점입니다.

```swift
let disposeBag = DisposeBag()
let observable1 = PublishSubject<Int>()
let observable2 = PublishSubject<String>()
let zippedObservable = Observable.zip(observable1, observable2)
```

위 `zippedObservable`의 타입은 `Observable<(Int, String)>`



```swift
let observable1 = PublishSubject<Int>()
let observable2 = PublishSubject<Int>()
let observables = [observable1, observable2]
let zippedObservable = Observable.zip(observables)
```

여기서 `zippedObservable`의 타입은 `Observable<[Int]>`



```swift
Observable.zip([publishSubject1, publishSubject2])
    .subscribe(onNext: { item in
        print(item)
    })
    .disposed(by: disposeBag)
    
publishSubject1.onNext(1)
publishSubject2.onNext(2) // [1, 2]
publishSubject1.onNext(3)
publishSubject2.onNext(4) // [3, 4]
publishSubject1.onNext(5)
```

`Observable<[Int]>`로 [Int] 형태의 값을 내보내지만, zip의 특성인 각 Observable의 일치하는 인덱스에서 방출하는 값들로 합쳐지는 것은 유지하고 있습니다.



따라서 zip은 튜플로도, 컬렉션 형태로도 처리될 수가 있다는 것을 확인할 수 있었습니다. 



<br>

#### References

- [ReactiveX - Merge](https://reactivex.io/documentation/operators/merge.html)
- [ReactiveX - Zip](https://reactivex.io/documentation/operators/zip.html)