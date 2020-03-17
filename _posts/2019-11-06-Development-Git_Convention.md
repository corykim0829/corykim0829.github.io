---
title: "[Git] commit msg guide 및 commit 방법"
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
date: 2019-11-06 12:00:00
lastmod: 2019-11-06 12:00:00
sitemap :
  changefreq : daily
  priority : 1.0
---

git commit message guide와 commit 방법



## Udacity Git Commit Message Style Guide (해석)

Udacity에서 사용하는 git commit msg style이다.

> Introduction

이 스타일 가이드는 여러분의 프로젝트에서 따라야할 공식적인 가이드처럼 사용될 것입니다. Udacity 평가자들은 이 가이드를 통해 여러분의 프로젝트를 평가할 것입니다. 개발의 세계에서는 "이상적인" 스타일에 대해 많은 의견들이 있습니다. 따라서, 학생들이 프로젝트를 진행하는데 있어서 어떤 스타일을 따라야하는지에 대한 혼란을 줄이기 위해서, 저희는 모든 학생들이 이 스타일 가이드를 따르기를 바랍니다.



> Commit Messages

#### 메세지 구조체: Message Structure

commit 메세지는 세개의 파트(:title, 선택적 body, 선택적 footer)로 빈줄에 의해 나뉘어져 있습니다. 구성은 다음과 같습니다 :

```
type: subject

body

footer
```

타이틀은 메세지의 타입과 주제로 구성되어있습니다.

#### 타입: the type

타입은 타이틀과 함께 포함되는데 아래 타입들 중 하나가 될 수 있다:

- feat: 새로운 기능
- fix: 버그를 고침
- docs: 문서를 수정
- style: 포맷, 빠진 세미 콜론 등; 코드 변경 X
- refactor: 제품 코드 리팩토링
- test: 테스트 추가, 테스트 리팩토링; 제품 코드 변경 X
- chore: 빌드 작업 업데이트, 패키지 매니저 config 등; 제품 코드 변경 X

chore는 이제까지 그냥 잡다한 작업을 처리하는 줄 알았는데, 다른 작업을 위한 type이었다.

#### 주제: the subject

주제는 50자를 넘으면 안되며, 대문자로 시작하고 마침표로 끝내면 안됩니다.

commit이 어떤 일을 하는지 서술하기 위해 어떤 것을 했다는 것 보다는 **명령적인 톤**을 사용합니다. 예를 들어, `chaged` 또는 `chages` 보다는 `change` 를 쓰는게 좋습니다.

#### The Body

모든 commit들이 body를 보증할 정도로 복잡하진 않으므로, body는 선택적이고 commit이 약간의 설명과 배경이 필요할 때에만 쓰인다. body는 commit이 무엇이고 왜 이 commit인지 설명하는데 쓰이지 어떻게 했는지를 스는 것이 아니다.

body를 작성할 때, title과 body사이의 공백줄이 필수적이고 72글자를 넘을 수 없도록 길이의 제한이 있다.

#### The Footer

footer는 선택적이며 issue tracker ID를 참조할 때 쓰인다.

####  Example Commit Message

```
feat: Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequenses of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789
```



## How to commit

여태까지 `git commit -m` 명령어를 사용하여서 간단하게 한줄로 요약하여 commit과 함께 메세지를 남겨왔다. 위에서 언급한 body와 footer도 같이 쓸 수 있는 방법을 찾아보았다.

`git add` 명령어를 통해 변경사항이 stage에 올라와있는 상태에서 아래와 같이 commit 명령어에 별다른 옵션을 주지 않고 `Enter`를 치면 된다.

```
$ git commit
```

여기서 vim으로 전환된다. git의 commit message struct 수정 deafult editor는 vim으로 설정되었다. 하지만 설정을 하면 다른 editor로도 수정이 가능하다.

vim은 에디터 중 하나로, 대표적인 모드는 다음과 같다.

- 일반 모드: `esc`
- 입력 모드: `i`
- 비주얼 모드: `v`
- ex 모드: `:`

<br>

처음에 vim에 들어가게 되면 일반 모드로 들어가지는데, 여기서 입력을 하기위해 i를 입력한다. 그러면 경로에서 아래와 같은 메세지가 가장 하단에 출력된다.

```
-- INSERT --
```

여기서 이제 위에서 나온 가이드라인에 맞게 title, body, footer를 공백 line으로 구분하여 작성을 하면 된다.

<br>

작성을 완료했다면, esc를 눌러 일반 모드로 돌아가서 `:wq` 를 입력하고 엔터를 치면 write & quite가 실행된다. 즉 저장하고 vim이 종료된다.

- `:q!` - 저장하지 않고 종료
- `:w!` - 파일을 덮어쓰기

<br>

정상적으로 저장하고 종료가 되었다면 commit을 push할 수 있게된다. push를 하면 github 에는 다음과 같이 log가 남는다.

<p align="left">
  <img src="/assets/images/2019-11-06-git-1.png" width="640px">
</p>

body와 footer가 추가되면 기존 커밋 log에서 더보기 버튼을 누르면 확인할 수 있다.



### References

- [Udacity Git Commit Message Style Guide](https://udacity.github.io/git-styleguide/)
- [Git의 기초 - 수정하고 저장소에 저장하기](https://git-scm.com/book/ko/v1/Git의-기초-수정하고-저장소에-저장하기)
- [vim기초](https://nolboo.kim/blog/2016/11/15/vim-for-beginner/)