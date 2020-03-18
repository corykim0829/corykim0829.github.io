---
title: "[Git] Rebase를 사용한 이미 커밋한 메세지 수정하기"
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

커밋 메세지를 실수 했을 때 대처하는 법

<br>

#### 들어가기 전에

버전관리 환경은 터미널을 사용하며, vi editor를 사용한다.

<br>

> 앗 커밋 메세지 실수했다!

커밋을 남기다 보면 가끔 실수를 할 때가 있는데, 바로 이전 커밋이면 쉽게 수정할 수 있지만, 여러개의 커밋 메세지를 실수 했다면 복잡해진다. 이 때 `rebase -i`를 사용하면 쉽게 메세지만을 변경할 수 있다.

<br>

### git rebase -i

```
-i
--interactive
Make a list of the commits which are about to be rebased. Let the user edit that list before rebasing. This mode can also be used to split commits (see SPLITTING COMMITS below).

The commit list format can be changed by setting the configuration option rebase.instructionFormat. A customized instruction format will automatically have the long commit hash prepended to the format.

See also INCOMPATIBLE OPTIONS below.
```

rebase될 커밋들의 리스트를 만든다. 사용자가 rebase하기 전에 리스트를 수정할 수 있게 해준다. 이 모드는 커밋을 쪼개는 데 사용할 수 있다.

커밋 리스트 포맷은 configuration 옵션의 rebase.instructionFormat 설정에 따라 바뀔 수 있다. 커스터마이즈된 명령 포맷은 긴 커밋 해쉬가 포맷 앞에 자동으로 붙습니다.

**Summary : ** 커밋 리스트를 가져와 수정하여 rebase를 할 수 있다는 말이다.
{: .notice--warning}

<br>

### 커밋 메세지 수정하기

커밋을 두번 남겼는데, 저장소의 issue number를 커밋에 담지 않아서 이 두개의 커밋 메세지를 수정해야 하는 상황이다.

현재 HEAD 부터 2개 까지의 커밋 이력을 수정하고 싶다면 터미널에서 다음과 같이 입력한다.

```bash
git rebase -i HEAD~2
```

그럼 다음과 같이 vi editor가 나오는데, 아래 Command 설명처럼 `pick`을 `reword`로 변경해준다.

커맨드 변경 후, `:wq`로 저장후 종료한다.

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

그럼 첫번째 커밋 메세지를 수정할 수 있도록 vi editor가 나오게 된다.
메세지를 수정했다면 마찬가지로 `:wq`를 사용하여 저장 후 종료

```
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

첫번째 커밋 메세지를 수정하고 저장 후 종료하면 다음과 같은 화면이 나온다.

<p align="left">
  <img src="/assets/images/git/rebase-i.png" width="700px">
</p>

당황하지 않고 위 설명에 나와있는대로 계속해서 rebase를 진행한다.

```
git rebase --continue
```

<br>

continue를 하게되면 첫번째 커밋 메세지를 수정한 화면이 다시 나오게 되는데, 동일하게 메세지를 수정해주고 `:wq`를 사용하여 저장과 종료를 해주면 된다.

<br>

#### References

- https://git-scm.com/docs/git-rebase