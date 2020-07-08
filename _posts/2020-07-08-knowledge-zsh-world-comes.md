---
title: "[MacOS Knowledge]바야흐로 zsh의 시대가 왔습니다"
excerpt: "bash 저리가라"

toc: true
toc_sticky: true

header:
  teaser: /assets/images/knowledge/knowledge-logo.jpg

categories:
  - knowledge
tags:
  - knowledge

last_modified_at: 2020-07-08T18:00:00+09:00
---

## 소개 

macOS 10.15인 Catalina부터 기본 쉘이 bash에서 zsh(Z shell)로 변경되었습니다!  
Git을 자주 사용하는 개발자들은 zsh을 이미 많이 사용해왔습니다.  
그 이유는 `branch`와 `git status`를 자동으로 확인할 수 있기 때문입니다.  

## bash보다 좋은 이유?  

- 기본 기능은 두개가 서로 비슷하지만, zsh은 코드를 추가하여 마우스를 지원합니다.
- 자동 제안 및 수정 기능을 제공하고, 문맥에 맞는 도움말을 지원합니다.
- 부동-소수점 수와 수학 함수 라이브러리를 제공합니다.  

## .zshrc 관련 

Mac OS X에는 deafault로 .zshrc가 존재하지 않습니다.  
따라서 구글링하면서 .zshrc에 관한 내용이 나오면 직접 만들어 주셔야 합니다.  

- `vi ~/.zshrc`

## 참고 문헌 

<https://xho95.github.io/macos/cli/shell/zsh/2020/03/04/Setting-Up-the-Zsh-shell-on-Mac.html>  

<https://superuser.com/questions/886132/where-is-the-zshrc-file-on-mac>  
