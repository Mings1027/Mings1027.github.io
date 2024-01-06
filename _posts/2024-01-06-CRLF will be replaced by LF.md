---
layout: post
title: Git 커밋 시 CRLF will be replaced by LF가 뜨는 경우
feature-img : "assets/img/post-img/beddingCat.jpg"
---

맥, 윈도우 개발자가 협업할 때 발생하는 문제라고 한다.

```
warning : CRLF will be replaced by LF ~~~
```
위와 같은 경고가 뜨는 경우가 있다. 혹은 반대일 수 있다.
이는 협업때 발생하는 Whitespace 에러라고한다. 유닉스 시스템은 한줄의 끝이 LF(Line Feed)로 이루어지는 반면, 윈도우에서는 줄하나가 CR(Carriage Return)와 LF(Line Feed) 즉 CRLF로 이루어지기 때문에 Git에서 어느쪽을 선택할지 혼란이 온다고 한다.

유닉스 OS를 쓰면 CRLF will be replaced by LF in... 이 뜰 것이고, 윈도우를 쓴다면 LF will be replaced by CRLF in... 라고 뜰 것이다.

## 해결 방법은 Git에 있는 기능인 core.autocrlf를 켜주는 것이다.

자동변환 해주는 기능이라고 한다.

이 기능은 개발자가 Git에 코드를 추가했을 때(커밋할 때)에는 CRLF를 LF로 변환해주고, Git코드를 개발자가 조회할 때(Clone한다거나 할때)에는 LF를 CRLF로 변환해준다.

그러므로 윈도우 개발자의 경우 이 변환이 항상 실행되도록 아래 명령어를 입력한다.

```
git config --global core.autocrlf true
```
해당 프로젝트에만 적용하고 싶다면 --global을 빼주면 된다.

리눅스나 맥 개발자의 경우 조회할 때 LF를 CRLF로 변환하는 것은 원하지 않을 수 있다. 따라서 뒤에 input을 추가해 단방향으로만 변환이 되게 설정한다.

```
git config --global core.autocrlf true input
```

혹은 변환을 원하지 않고, 그냥 warning 메시지를 끄고 알아서 하고싶은 경우 이 기능을 꺼주면 된다.

```
git config --global core.autocrlf false
```