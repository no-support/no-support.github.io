---
layout: single
title: "git stash 특징 정리"
categories:
  - git
tags: [stash]
# toc: true
author_profile: true
# search: false
sidebar:
  - nav: "docs"
---
# stash의 사용처 #
 다른 브랜치로 이동할 때(브랜치로 이동하기 전 반드시 commit을 해야 하는데, commit 하기에는 하나의 기능을 구현 완료하지 못 했을 때)

## git stash 주요 명령어 ##
 - git stash: 임시 저장하기
 - git stash list: stash 목록 보기
 - git stash save "2022/12/20 feature->hot-fix로 이동 전 저장": 네이밍을 해주지 않을 시 인덱스 번호로 관리되기 때문에(최근 stash된 것이 index 0번에  - 저장됨) 네이밍할 것을 추천
 - git stash pop [--index ]: stash 꺼내기
 - git stash drop: stash 삭제하기
 - git stash clear: stash 목록 모두 지우기
