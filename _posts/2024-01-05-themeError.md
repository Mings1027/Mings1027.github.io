---
layout: post
title: 블로그 테마 적용 오류
feature-img : "assets/img/post-img/cat_surprised_look.jpg"
date : January 5th, 2024
---

{% include aligner.html images="post-img/cat_surprised_look.jpg" column=1 %}

마음에 드는 테마를 찾아 적용하고 로컬서버에서 확인했을 때는 문제 없었지만 git push 후 본인의 블로그에 접속했을 때 아래 사진처럼 css가 적용되지 않는 문제가 생겼다면 아래 방법을 시도해보시기 바랍니다.

{% include aligner.html images="post-img/error.png" column=1 %}

## 해결 방안 1 _Config.yml 파일 baseurl, url 수정
가장 많은 방법이라고 한다. github 블로그를 clone 하는 과정에서 기존에 작성된 경로와 본인의 경로가 다른 경우 발생할 수 있다고 한다.

baseurl은 공백으로, url은 본인 블로그 주소로 변경해준다.

```
baseurl : ""
url : https://username.github.io"
```

## 해결 방안 2 _Config.yml 파일 내 remote_theme 부분 주석 해제
만약 주석 처리 되어 있다면 github server에 제대로 호스팅이 되지 않을 수 있다고한다.