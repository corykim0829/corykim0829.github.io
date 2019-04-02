---
title: "Algorithm April 1st 2019"
header:
  teaser: /assets/images/algorithm-logo.png
  overlay_image: /assets/images/algorithm-logo-horizon.png
  overlay_filter: 0.2
categories:
  - Algorithm
tags:
  - Swift
  - Algorithm
---



> [problem 10988 펠린드롬인지 확인하기](https://www.acmicpc.net/problem/10988)

```swift
import Foundation

let word: String = readLine()!

extension String {
    subscript (i: Int) -> Character {
        return self[index(startIndex, offsetBy: i)]
    }
}

func solution(_ word: String) -> Int {
    let maxIndex = word.count
    for i in 0..<maxIndex/2 {
        if word[i] != word[maxIndex - (i+1)] {
            return 0
        }
    }
    return 1
}

print(solution(word))
```



### extension, subscript

using extension to access nth index of string

```swift
extension String {
    subscript (i: Int) -> Character {
        return self[index(startIndex, offsetBy: i)]
    }
}
```

subscript let us can access code with bracket `[]`

[string extensions in StackOverflow](https://stackoverflow.com/questions/24092884/get-nth-character-of-a-string-in-swift-programming-language)



> [problem 1018 체스판 다시 칠하기](https://www.acmicpc.net/problem/1018)

8 * 8 의 체스판 만들기

문제를 잘못이해하여 다시 접근해야한다.