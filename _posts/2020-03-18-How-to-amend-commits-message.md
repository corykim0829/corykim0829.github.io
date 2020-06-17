---
title: "[Git] Rebase를 활용해 이미 커밋한 메세지 수정하기"
header:
  teaser: /assets/images/related-to-development.jpg
  overlay_image: /assets/images/related-to-development-h.jpeg
  overlay_filter: 0.2
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - Git
tags:
  - rebase -i
  - Git
  - Commit message
date: 2020-03-18 12:37:55
lastmod: 2020-03-18 12:37:55
sitemap :
  changefreq : daily
  priority : 1.0
---

커밋 메세지를 실수 했을 때 대처하는 법을 공유해보려고 합니다.

<br>

#### 들어가기 전에

커밋은 개발에서 거의 뗄 수 없는 친구이며 애증의 관계입니다. 또한 프로젝트를 시작하게 되면 커밋 컨벤션을 지키면서 커밋을 해야합니다. 이와 같은 개발 환경에서 실수로 컨벤션을 지키지 못하고 커밋을 했다거나, 커밋 메세지에 오타가 들어간 경우와 같이 수정이 필요한 경우가 생깁니다. 바로 직전의 커밋을 고쳐야한다면 쉽게 해결할 수 있지만, 몇번 커밋을 하고 나서야 이전 커밋의 오류를 발견할 때도 있죠. 거기다 이를 `push`까지 해버렸다면 상당히 골치아파집니다. 🤯 이러한 상황에 조금 `git` 명령어 중 `rebase`를 활용하여 쉽고 간편하게 커밋 메세지를 수정할 수 있는 방법을 작성해보려고 합니다. 

- **버전관리 환경**은 `터미널`을 사용했습니다.
- `vi editor`를 사용하여 **커밋 메세지를 수정**했습니다.

<br>

### git rebase -i

먼저 `git rebase -i`에 대해서 알아보도록 하겠습니다.
git 문서에는 이와 같이 명시되어있습니다.

```
-i
--interactive
Make a list of the commits which are about to be rebased.
Let the user edit that list before rebasing.
This mode can also be used to split commits (see SPLITTING COMMITS below).

The commit list format can be changed by setting the configuration option rebase.instructionFormat.
A customized instruction format will automatically have the long commit hash prepended to the format.

See also INCOMPATIBLE OPTIONS below.
```

rebase될 커밋들의 리스트를 만듭니다. 사용자가 rebase하기 전에 리스트를 수정할 수 있게 해준다. 이 상태에서는 커밋을 쪼개는 데 사용할 수 있다.

커밋 리스트 포맷은 configuration 옵션의 rebase.instructionFormat 설정에 따라 바뀔 수 있습니다. 커스터마이즈된 명령 포맷은 긴 커밋 해쉬가 포맷 앞에 자동으로 붙습니다.

요약 : 커밋 리스트를 가져와 수정하여 rebase를 할 수 있습니다.
{: .notice--warning}

<br>

### 모든 커밋 메세지 수정하기

두개의 커밋을 남겼는데, 커밋 컨벤션인 `깃헙 저장소의 issue number`를 커밋 메세지 앞에 명시하지 않아서 이 두개의 커밋 메세지를 수정해야 하는 상황이라고 가정하겠습니다.

현재 HEAD 부터 n개 까지의 커밋 이력을 수정하고 싶다면 터미널에서 다음과 같이 입력하면 됩니다.

```bash
git rebase -i HEAD~n
```

2개의 커밋을 수정하고 싶기 때문에 n에 2를 넣고 명령어를 입력하게 되면 다음과 같이 vi editor가 나오게 됩니다.
커밋 메세지는 n에 입력한 만큼 커밋 메세지가 `Command`와 함께 열거됩니다. Default 값으로 `pick` 이 들어가 있습니다.

하단 부분에는 `Commands`의 종류와 설명이 나타나있습니다. 여기서 우리는 `edit`를 사용하여 메세지를 수정할 것입니다. 
현재 vi editor이기 때문에 `i`를 누르고 `pick`을 `edit`로 수정해주고 `esc` - `:wq`로 저장 후 종료 합니다.

```
pick cb904d1 Feat: DoodleDataManager - fetching & decoding JSON
pick e2700a0 Refactor: Sort swift files by their function

% Rebase db0b078..e2700a0 onto db0b078 (2 commands)
%
% Commands:
% p, pick <commit> = use commit
% r, reword <commit> = use commit, but edit the commit message
% e, edit <commit> = use commit, but stop for amending
% s, squash <commit> = use commit, but meld into previous commit
% f, fixup <commit> = like "squash", but discard this commit's log message
% x, exec <command> = run command (the rest of the line) using shell
% b, break = stop here (continue rebase later with 'git rebase --continue')
% d, drop <commit> = remove commit
% l, label <label> = label current HEAD with a name
% t, reset <label> = reset HEAD to a label
% m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
% .       create a merge commit using the original merge commit's
% .       message (or the oneline, if no original merge commit was
% .       specified). Use -c <commit> to reword the commit message.
%
% These lines can be re-ordered; they are executed from top to bottom.
%
% If you remove a line here THAT COMMIT WILL BE LOST.
%
% However, if you remove everything, the rebase will be aborted.
%
% Note that empty commits are commented out
```

<br>

그럼 먼저 **첫번째 커밋 메세지**를 수정할 수 있도록 vi editor가 나오게 됩니다. 마찬가지로 vi editor이며 수정하여 원하는 메세지로 수정합니다.
메세지를 수정했다면 마찬가지로 `:wq`를 사용하여 저장 후 종료합니다.

```s
[#7] Feat: DoodleDataManager - fetching & decoding JSON

% Please enter the commit message for your changes. Lines starting
% with '%' will be ignored, and an empty message aborts the commit.
%
% Date:      Wed Mar 18 10:59:18 2020 +0900
%
% interactive rebase in progress; onto db0b078
% Last command done (1 command done):
%    reword cb904d1 [#7] Feat: DoodleDataManager - fetching & decoding JSON
% Next command to do (1 remaining command):
%    reword e2700a0 [#7] Refactor: Sort swift files by their function
% You are currently editing a commit while rebasing branch 'feature-imageUrl-json' on 'db0b078'.
%
% Changes to be committed:
%       modified:   PhotosApp/PhotosApp.xcodeproj/project.pbxproj
%       new file:   PhotosApp/PhotosApp/Models/DoodleDataManager.swift
%
```

<br>

정상적으로 첫번째 커밋이 수정됐다면 다음과 같은 화면이 나옵니다.

<p align="left">
  <img src="/assets/images/git/rebase-i-1.png" width="1000px">
</p>



hint에 나와있는 설명대로 계속해서 두번째 커밋 메세지도 수정하고 싶다면 아래 명령어를 입력하시면 됩니다. 그러면 첫번째 커밋 메세지를 수정할 때 나온 화면이 동일하게 나타나며 같은 방법을 수정하시면 됩니다.

```
git rebase --continue
```

<br>

### 특정 커밋 메세지 수정하기

`get rebase -i HEAD~n` 명령어를 통해 나온 커밋들 중에 하나만 수정한다고 가정해보겠습니다.

위와 마찬가지로 `pick` command가 기본으로 정해진 커밋 리스트들이 명시될 것이고 이 중에서 하나의 커밋만 `edit`로 바꾸고 저장 후 종료를 하게 되면 아래와 같은 문구가 뜨게 됩니다.

<p align="left">
  <img src="/assets/images/git/rebase-i-2.png" width="1000px">
</p>
친절하게 설명이 잘 나와있기 때문에 수정을 계속해서 진행하기 위해 아래 명령어를 입력하면 됩니다.

```
git commit --amend
```

수정을 마치고 저장 후 종료를 했다면, 위 이미지에 설명이 되어있는 것처럼 아래 명령어로 `rebase`를 마무리하면 됩니다.

```
git rebase --continue
```

<br>

### 수정된 커밋 메세지 원격 저장소에 반영

수정을 완료해도 깃헙 저장소의 커밋 이력은 변경되지 않았나요? 현재까지 진행한 것은 **로컬**에서 커밋 메세지를 수정한 것이었습니다. **원격**에 있는 이력을 바꾸기 위해서는 `--force` 옵션을 사용해 `push`해야합니다. 명령어는 다음과 같습니다.
두개 모두 동일한 명령어이며 `--`는 축약어를 사용하지 않겠다는 의미입니다.

아래 명령어를 실행하면 로컬에서 master 브랜치에서 수정된 커밋 메세지가 원격까지 모두 반영됩니다.

```
git push --force origin master
```

```
git push -f origin master
```

<br>

#### 마치며

rebase는 깃에 익숙하지 않으면 잘 와닿지 않는 명령어 중 하나입니다. 하지만 rebase를 잘 활용할 수 있다면 여러가지 작업을 편리하게 사용할 수 있다는 장점이 있습니다. 이번 포스트에서는 rebase를 활용하여 커밋 메세지를 수정하는 방법을 알아보았습니다. git은 어려워 보이지만 항상 침착하게만 한다면 잘 해결되는 것 같습니다. 두려우시면 브랜치를 따로 파서 시도해보는 것을 추천합니다! 🤖

<br>

#### References

- [git-rebase](https://git-scm.com/docs/git-rebase)