---
layout: post
title: 맥에서 Ruby 와 Jekyll 설치하고 로컬 환경에서 포스트 미리 확인하는 방법
feature-img : "assets/img/post-img/cameraCat.jpg"
---



## 1. Homebrew 설치
맥에는 Homebrew라는 강력한 패키지 관리자가 있다. Ruby나 Jekyll을 쉽게 설치하기 위해 이것을 먼저 설치해준다.
터미널에서 아래 명령어를 적는다.
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
중간중간 RETURN을 누르라거나 비밀번호를 입력하라는 글이 나오면 시키는대로 해준다.

기다리다 보면 Next Step 이라면서 글이 쭉 나온다. 그때 아래 명령어 같은것이 보일건데
```
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"'
eval "$(/opt/homebrew/bin/brew shellenv)"
``` 
두개 중 위에 것은 echo 만 지우고 입력하고 아래는 그대로 적어준다.

설치가 잘 되었는지 아래 명령어로 확인해본다.
```
brew -v
```

## 2. Ruby 설치
```
brew install rbenv ruby-build
```
위 명령어를 적어준다.
brew install 뒤에 패키지명이 오는데 이 경우 rbenv와 ruby-build 두개를 설치하기 위해 같이 적어준 것이다.

시간이 좀 걸릴 수 있다. 다 되었다면 설치 가능한 버전들을 아래 명령어로 확인해본다.
```
rbenv install -l
```
최신버전은 호환문제가 생길 수 있으니 나오는 버전들 중 제일 위에서 한두개 아래것을 선택해준다.
작성자 본인은 아래 버전을 설치했다.
```
rbenv install 3.1.4
```
설치 후 아래 명령어로 버전을 확인해본다.
```
rbenv versions
```
```
system
* 3.1.4 (set by /Users/mings/.rbenv/version)
```
아래와 같이 나올건데 * 표시가 system 앞에 있을 수 있다. 이때 아래 두 명령어를 순서대로 적어준다.
```
rbenv global 3.1.4
rbenv rehash
```
global 뒤에는 당연히 본인이 설치한 버전을 넣어주어야한다.
다시 버전을 확인해보면 위 사진처럼 * 표시가 아래로 내려가 있을것이다.

## 3. Jekyll 설치 및 에러
아래 명령어를 적어준다.
```
gem install bundler 
```
아래와 같은 에러가 뜬다면
```                                       
Fetching bundler-2.4.19.gem
ERROR:  While executing gem ... (Gem::FilePermissionError)
You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.
```
rbenv의 PATH를 추가해주면 된다.
우선 아래 명령어로 텍스트 파일을 열어준다.
```
open ~/.zshrc
```
쭉 내려서 아래에 두줄을 추가해준다. 
```
export PATH={$Home}/.rbenv/bin:$PATH && \
eval "$(rbenv init -)"
```
터미널에 적는게 아니고 방금 열린 텍스트 파일 안에 적어야한다.

저장해주고 터미널로 돌아와서 아래 명령어를 적으면 파일이 적용된다.
```
source ~/.zshrc
```
이제 아래 명렁어를 다시 입력해본다.
```
gem install bundler 
```
해주면
```
Fetching bundler-2.4.19.gem
Successfully installed bundler-2.4.19
Parsing documentation for bundler-2.4.19
Installing ri documentation for bundler-2.4.19
Done installing documentation for bundler after 1 seconds
1 gem installed
```

1 gem installed 보이면 잘 설치된것이다.

이제 Jekyll을 install 해준다.
```
gem install jekyll
```
만약 에러가 뜬다면 아래 명령어로 Command Line Tools를 재설치 해본다.
```
xcode-select --install
```
만약 설치되어있다는 에러가 뜨면 아래 명령어로 지우고 설치해본다.
```
sudo rm -rf /Library/Developer/CommandLineTools
xcode-select --install
```

완료 후 다시 아래 명령어를 적어준다.
```
gem install jekyll
```

마지막이다. 자신의 github.io 디렉토리로 이동 후 Gemfile이 있는지 확인해보자 이 파일이 있는 위치에서 아래 명령어를 순서대로 적어준다.
```
bundler install
bundle exec jekyll serve
```
로컬 서버가 열리고 쭉 뜰텐데 아래와 같은 글이 보이면 server address 뒤 링크를 복사해 브라우저 주소창에 넣어준다.
```
Server address: http://127.0.0.1:4000
Server running... press ctrl-c to stop.
```
본인 블로그가 뜰거고 로컬에서 보이는 것이기 때문에 지금 당장 여기 보인다고 실제 깃허브 블로그에 반영이 된 것이 아니다.

반영시키려면 git push 해주면 된다.

서버 닫는 방법은 
```
Server running... press ctrl-c to stop.
```
server address 밑에 적혀있다.