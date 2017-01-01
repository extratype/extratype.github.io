---
title: MSYS2 Git 사용하기
date: 2016-10-16 00:59:55
categories: coding
tags:
- MSYS2
- Git
---
**주의: 언급한 에러 외에도 잔버그가 여럿 있으니 권장하지 않음!!**

## Windows에서 Git 사용하기
[Git for Windows](https://git-for-windows.github.io/)를 설치하면 자체 MSYS2 환경도 같이 설치된다. Git Bash가 바로 이 환경에서 동작한다.

이미 [MSYS2](https://msys2.github.io/)를 설치했다면 기존 MSYS2 환경에 덮어서 설치할 수 [있다](https://github.com/git-for-windows/git/wiki/Install-inside-MSYS2-proper). 이 [issue](https://github.com/git-for-windows/git/issues/284)에 따르면 두 환경이 아직 통합되지 않은 것 같다.

나는 아예 MSYS2 Git(`git`)을 설치해서 사용하고 있는데, Git 공식이 아니어서 그런지 Git을 쓰는 다른 프로그램들이 오류를 뱉는 경우가 있었다. 대처법을 여기에 정리해 올린다.

## Visual Studio Code
Git을 쓰려고 하면 `ENOENT` 에러를 뱉는다. 관련 issue는 [여기](https://github.com/Microsoft/vscode/issues/7998). 링크엔 Cygwin 얘기만 나오는데 MSYS2에선 아래처럼 symlink를 만들면 된다.
```
C:\> mklink c c:\
```
`/D`나 `/J` 옵션을 붙여주지 않아도 MSYS2라 그런지 잘 된다.

## Atlassian SourceTree
\[터미널\]을 누르면 뻗어버린다. \[옵션\]->\[Git\]에서 Git Bash 사용 옵션을 끄면 된다. HTTPS로 푸시하려고 하면 잘 안 되는데 MSYS2에서 OpenSSH 설치해서 쓰면 잘 된다. GitHub에 푸시하는 방법은 [여기](https://help.github.com/articles/error-permission-denied-publickey/)를 참고.