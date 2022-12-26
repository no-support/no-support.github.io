---
layout: single
title: "git 주요 명령어 정리"
categories:
  - git
tags: [stash]
# toc: true
author_profile: true
# search: false
sidebar:
  - nav: "docs"
---
# stash #
 사용처: 다른 브랜치로 이동할 때(브랜치로 이동하기 전 반드시 commit을 해야 하는데, commit 하기에는 하나의 기능을 구현 완료하지 못 했을 때)

## git stash 주요 명령어 ##
 - git stash save "2022/12/20 feature->hot-fix로 이동 전 저장": 네이밍을 해주지 않을 시 인덱스 번호로 관리되기 때문에(최근 stash된 것이 index 0번에  - 저장됨) 네이밍할 것을 추천
 - git stash: 네이밍하지 않고 임시 저장하기
 - git stash list: stash 목록 보기
 - git stash pop [--index ]: 최근 stash 꺼내기, --index {n}번 째 stash 꺼내기
 - git stash drop &lt;index&gt;: 해당 stash 삭제하기
 - git stash clear: stash 목록 모두 지우기

 # commit --amend 옵션 #
 사용처: 최근 커밋 메시지 변경
 특징: 최근 커밋의 해시 코드가 변경됨
 ## git 사용례 ##
  - 최근 커밋에 새 작업을 하거나 하지 않은 상태에서 git commit --amend -m "새 커밋 메시지" -> 기존 커밋이 새 커밋 메시지로 덮어짐




git switch <hashcode> --detach: 돌아가서 잠깐 보기만 하고 오려는 용도. head가 브랜치를 참조하지 않고 커밋 버전을 직접 참조하고 있기 때문에 bash 프롬프트에 ((해시코드...)) 이렇게 보인다.
git switch main: 돌아오기. 이 때 보기만 하고 오려다가 수정도 했다면 돌아갈 수 없고(Aborting), 커밋을 하거나 stash로 보관하라고 뜸
 - git commit -am "switch --detach된 상태에서 작업함" -> 새 커밋 아이디 생성되며 bash 프롬프트에 ((새 해시코드...)) 이렇게 보인다. 즉 head가 새 커밋을 참조하게 된다.
   이후 git switch main으로 돌아왔을 때 새 해시코드가 git log --oneline 기록에 없다. main 브랜치로 이동한 상태에서 git merge <hashcode>를 통해 병합하여 해결한다. 
   깃 로그 내역은 '이전 커밋 -> switch --detach해서 만든 커밋 -> 둘을 병합하며 생성된 새 커밋'으로 된다.

git restore <a.txt>: a.txt 파일 최신 커밋 버전으로 복구
git restore --source <hashcode> <a.txt>: a.txt 파일을 해당 커밋 버전으로 복구


git restore --staged a.txt: 스테이징되었던 a.txt파일 스테이징 취소


revert: 새 커밋을 만들며 이전 버전으로 돌아감
예시)
로그 상황: a(initial commit) - b - c - d(head->main)
git revert b: 새 커밋(e) 생성되며(a - b - c - d - e), 이전으로 즉 a로 돌아감(돌아가면서 d와 a 버전에 충돌이 있을 수 있으며, 충돌이 없다 해도 revert의 특성 상 새 커밋이 생김).


reset은 head가 참조하던 브랜치가 참조하던 커밋을 바꾸는 것이다.
a(initial commit) - b - c - d(head->main)일 때, git reset --hard b을 했다 해도, git reset --hard c또는 d로 다시 앞으로 갈 수 있고, d로 왔을 때 git log --oneline으로 보면 예전과 같다.
만약 reset --hard b에서 새 커밋을 생성(e)하였다면, git reset --hard d를 한 후 git merge할 수 있다.(f). 깃 로그로 확인해보면 a - b - c - d - e - f로 나와있다.