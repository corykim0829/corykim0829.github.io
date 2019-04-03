---
title: "Algorithm April 3rd 2019"
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



> [problem 2941 크로아티아 알파벳](https://www.acmicpc.net/problem/2941)

```swift
import Foundation

var input: String = ""

func getInput() {
    input = readLine()!
}

func solution(_ inputString: String) -> Int {
    var input = inputString
    let CroatiaAlphabet = ["c=", "c-", "dz=", "d-", "lj", "nj", "s=", "z="]
    
    func replaceCroatiaAlphabetWithNoneCroatian() {
        for alphabet in CroatiaAlphabet {
            input = input.replacingOccurrences(of: alphabet, with: "A")
        }
    }
    
    replaceCroatiaAlphabetWithNoneCroatian()
    return input.count
}

getInput()
print(solution(input))
```

너무 고정관념처럼 생각하지말고 조금 더 유동적으로 생각하면 좋을 것 같다.

조금 더 문제에 집중해야할 듯



이번 문제의 핵심 함수



### replacingOccurrences(of:with:)

Returns a new string in which all occurrences of a target string in the receiver are replaced by another given string.