---
title: "[Common sense]Homebrew 2.4.0 Update"
excerpt: "macOS 용 패키지 관리자, MikeMcQuaid님의 설명"

toc: true
toc_sticky: true

categories:
  - knowledge
tags:
  - knowledge

last_modified_at: 2020-07-08T19:30:00+09:00
---

## 소개  

오늘은 Homebrew 2.4.0에 대해서 공지하겠습니다.  
2.3.0 버전 이후 가장 중요한 변화는 macOS Maverics의 지원 중단, `devel` 버전의 사용 중단과 `brew audit`의 속도 향상입니다.  

## 2.3.0 버전 이후 주요 변화와 사용 중단 

- Homebrew's minimum macOS version is Yosemite(OS X 10.10의 이름).
- `devel` versions in formulae have been deprecated.
- `brew pull` has been deprecated in favour of `hub checkout`.  

## 2.3.0 버전 이후 기타 변경 사항  

- `brew audit` is significantly faster.
- `brew irb` correctly uses TERMINFO to improve keyboard handling.
- Homebrew uses system Gems correctly.
- `man brew` lists the full help for all official external commands  
(which now includes `brew test-bot`).  

## 결론 

- Hombrew accepts donations through GitHub Sponsors and still accepts donations through Patreon. If you can afford it, please consider donating. If you'd rather not use Github Sponsors or Patreon(our preferred donation methods), check out the other ways to donate in our README.

## References(참고 문헌)    

<https://brew.sh/2020/06/11/homebrew-2.4.0/>  
