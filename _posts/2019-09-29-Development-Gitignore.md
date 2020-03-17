---
title: "[Git] remote에 push 되버린 파일에 .gitignore 적용하기"
header:
  teaser: /assets/images/related-to-development.jpg
  overlay_image: /assets/images/related-to-development-h.jpeg
  overlay_filter: 0.2
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - Development
tags:
  - Development
  - Git
layout: post
date: 2019-09-29 12:00:00
lastmod: 2019-09-29 12:00:00
sitemap :
  changefreq : daily
  priority : 1.0
---



### git해본 사람이라면 다 아는 gitignore

대부분 gitignore를 알지만, 갑자기 적용하려니 생각이 안날 때가 있다. github에서 제공하는 기본적인 gitignore를 적용하며 개발을 진행하다가 갑작스럽게 gitignore를 추가해야하는 경우나 실수로 repository에 올라가버린 파일들에게도 gitignore적용이 필요할 때가 있다.



### gitignore 작성법

1. 아무것도 없는 라인이나, #로 시작하는 라인은 무시한다(주석).
2. 표준 Glob 패턴을 사용한다.
3. `/` 로 시작하면 하위 디렉터리에 적용되지 않는다.
4. 디렉터리는 `/` 를 끝에 사용하는 것으로 표현한다.
5. 느낌표`!` 로 시작하는 패턴의 파일은 무시하지 않는다.



Glob패턴은 정규표현식을 단순하게 만든 것으로 생각하면 되고 보통 셸에서 많이 사용한다.
애스터리스크(*)는 문자가 하나도 없거나 하나 이상을 의미하고, [abc]는 중괄호 안에 있는 문자 중 하나를 의미한다. 등



### 예시

```
## Build generated
build/
DerivedData/

## Various settings
*.pbxuser
!default.pbxuser
```

`## Various settings` 에서 확장자가 `.pbxuser` 인 파일들을 무시하겠지만, `default.pbxuser` 는 무시하지 않는다.



```
fastlane/screenshots/**/*.png
```

`fastlane/screenshots/` 디렉토리 안의 모든 `.png` 파일을 무시한다.



### 이미 올라간 파일에 적용법

git rm을 알아야한다. Tracked 상태의 파일을 삭제한 후에 

git rm --cached 



git rm -r

git rm -r --cached



참고자료

- [프로 Git 2판](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=79232604)