---
layout: post
title: 블로그 테마 적용 오류
feature-img : "assets/img/post-img/cat_surprised_look.jpg"
---

{% include aligner.html images="post-img/cat_surprised_look.jpg" column=1 %}

## 해결 방안 1 _Config.yml 파일 baseurl, url 수정
가장 많은 방법이라고 한다. github 블로그를 clone 하는 과정에서 기존에 작성된 경로와 본인의 경로가 다른 경우 발생할 수 있다고 한다.

baseurl은 공백으로, url은 본인 블로그 주소로 변경해준다.

```
baseurl : ""
url : https://username.github.io"
```

## 해결 방안 2 _Config.yml 파일 내 remote_theme 부분 주석 해제
만약 주석 처리 되어 있다면 github server에 제대로 호스팅이 되지 않을 수 있다고한다.