---
title: "[iOS] Test Case와 Test Method"
header:
  teaser: /assets/images/ios-posts2.jpeg
  overlay_image: /assets/images/ios-posts2.jpeg
  overlay_filter: 0.2
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - iOS
tags:
  - iOS
---

유닛테스트에 사용되는 테스트 케이스와 테스트 메소드



### 들어가기 전에

> Assertion

컴퓨터 프로그래밍에서 표명(表明), 가정 설정문(假定設定文) 또는 어서션(영어: **assertion**)은 프로그램 안에 추가하는 참·거짓을 미리 가정하는 문이다. 개발자는 해당 문이 그 문의 장소에서 언제나 참이라고 간주한다. 런타임 중에 표명이 거짓으로 평가되면 표명 실패(assertion failure)를 초래하며 이 상황에서는 일반적으로 실행이 중단된다.

출처 : [위키백과](https://ko.wikipedia.org/wiki/%ED%91%9C%EB%AA%85)

해석하면 Assertion은 표명이라는 말과 매핑된다. 하지만 표명이라는 단어는 친숙하지 않은 단어이기 떄문에 번역에서 Assertion 영어 그대로 사용할 예정이다.

<br>

[원문](https://developer.apple.com/documentation/xctest/defining_test_cases_and_test_methods)

## Defining Test Cases and Test Methods

작성한 코드가 예상대로 동작하는 지를 확인하기 위해 **test case** 와 **test method**를 타겟에 추가할 수 있다.

---

### 개요

Xcode 프로젝트에 테스트 메소드를 하나 이상 작성하여 테스트를 추가한다. 각 메소드는 코드의 특정 측면들을 확인할 수 있다. 관련된 테스트 메소드들을 테스트 케이스들로 묶어라. 각 케이스들은 XCTestCase의 subclass이다.

프로젝트에 테스트를 추가하는 법

- 테스트 타겟 안에 XCTestCase의 subclass를 생성한다.
- 하나 이상의 테스트 메소드를 테스트 케이스에 작성한다
- 하나 이상의 테스트 assertions를 테스트 메소드에 추가한다.

테스트 메소드는 XCTestCase subclass의 인스턴스 메소드이며, 파라미터가 없고, 리턴값이 없으며 소문자 `test` 로 시작된다. 테스트 메소드는 Xcode의 **XCTest framework**가 자동적으로 탐색되어진다.

```swift
class TableValidationTests: XCTestCase {
    /// Tests that a new table instance has zero rows and columns.
    func testEmptyTableRowAndColumnCount() {
        let table = Table()
        XCTAssertEqual(table.rowCount, 0, "Row count was not zero.")
        XCTAssertEqual(table.columnCount, 0, "Column count was not zero.")
    }
}
```

이 예제는 XCTestCase의 subclass인 `TableValidationTests`를 정의했다. 이 클래스는 테스트 메소드 `testEmptyTableRowAndColumnCount()` 하나를 가지고 있다. 이 테스트 메소드는 `Table` 클래스의 새로운 인스턴스를 생성하고, 이 인스턴스기 초기화 된 후 `rowCount`와 `columnCount` 프로퍼티가 모두 값이 0인지 확인한다.



> **Tip**
>
> 테스트 케이스와 테스트 메소드의 이름은 Xcode의 test navigator와 intergration reports에서 테스트들을 그룹화하고 식별하는 데 사용된다.
>
> 테스트 조직을 명확히하기 위해서, 각 테스트 케이스에 어떤 테스트가 안에서 진행되는지 알 수 있도록 이름을 요약하는게 좋다. (`TableValidationTests`, `NetworkReachabilityTests`, or `JSONParsingTests`.)
>
> 테스트 실패를 식별하기 위해서, 마찬가지로 각 테스트 메소드의 이름을 명확하게 한다. (`testEmptyTableRowAndColumnCount()`, `testUnreachableURLAccessThrowsAnError()`, or `testUserJSONFeedParsing()`.)



### Asserting Test Conditions

작성된 코드가 예상한대로 동작되는지 확인하기 위해 테스트 메소드 안에 **조건들**을 확인 또는 assert할 수 있다. `XCTAssert` 함수 계열은 Boolean 조건, nil 또는 nil이 아닌 값, 예상 값 또는 발생한 에러들을 확인하기 위해 사용할 수 있다.

위의 예시 코드에서는 두개의 Integer값이 같은 값을 가지고 있는지 assert하는 함수인 `XCTAssertEqual(_:_:_:file:line:)`를 사용했다.

---

## Test Methods

테스트 케이스에서 사용되는 테스트 메소드, 필요한 expression 만큼 왼쪽부터 파라미터를 채워가면 된다. 나머지 파라미터들은 기본 값을 가지고 있다.

#### Boolean Assertions

`expression` 1개 필요

- `XCTAssert(_:_:file:line:)`
- `XCTAssertTrue(_:_:file:line:)`
- `XCTAssertFalse(_:_:file:line:)`

#### Nil and Non-nil Assertions

`expression` 1개 필요

- `XCTAssertNil(_:_:file:line:)`
- `XCTAssertNotNil(_:_:file:line:)`

#### Equality and Inequality Assertions

`expression` 2개 필요, Accuracy 메소드는 `accuracy` 파라미터 필수

- `XCTAssertEqual(_:_:_:file:line:)`

  - ```swift
    class FrameSizeTests: XCTestCase {
        func testInitialFrameSize() {
            let view = CustomView()
            let expectedSize = CGSize(width: 640.0, height: 480.0)
            XCTAssertEqual(view.frame.size, expectedSize, "Unexpected frame size.")
        }
    }
    ```

- `XCTAssertNotEqual(_:_:_:file:line:)`

- `XCTAssertEqualWithAccuracy(_:_:accuracy:_:file:line:)`

- `XCTAssertNotEqualWithAccuracy(_:_:accuracy:_:file:line:)`

#### Comparable Value Assertions

`expression` 2개 필요

- `XCTAssertGreaterThan(_:_:_:file:line:)`
- `XCTAssertGreaterThanOrEqual(_:_:_:file:line:)`
- `XCTAssertLessThan(_:_:_:file:line:)`
- `XCTAssertLessThanOrEqual(_:_:_:file:line:)`

#### Error Assertions

- `XCTAssertThrowsError(_:_:file:line:_:)`
- `XCTAssertNoThrow(_:_:file:line:)`

