---
title: "맥에서 Ruby 와 Jekyll 설치 방법과 로컬 환경에서 포스트 미리 확인하는 방법"
excerpt: "github.io 를 만들고 포스트를 로컬에서 확인하기 위해 ruby, jekyll을 설치해보자 "

categories: Blog
tags:
  - [git, github, ruby, jekyll]

toc: true
toc_sticky: true
author_profile: false

sidebar:
    nav: "docs"

date: 2023-09-03
last_modified_at: 2023-09-03
---






# 1. Homebrew 설치
맥에는 Homebrew라는 강력한 패키지 관리자가 있다. 설명은 생략하고 설치방법을 적어보려한다.
터미널에서 아래 명령어를 적어보자.
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
중간중간 RETURN을 누르라거나 비밀번호를 입력하라는 글이 나올 수 있다 시키는데로 해주자.

기다리다 보면 Next Step 이라면서 글이 쭉 나올거다 그때 아래 명령어 같은것이 보일거다
```
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"'
eval "$(/opt/homebrew/bin/brew shellenv)"
``` 
두개 중 위에 것은 echo 만 지우고 입력하고 아래는 그대로 적어주자.

설치가 잘 되었는지 아래 명령어로 확인해본다.
```
brew -v
```

# 2. Ruby 설치
```
brew install rbenv ruby-build
```
위 명령어를 적어주자.
brew install 뒤에 패키지명이 오는데 이 경우 rbenv와 ruby-build 두개를 설치하기 위해 같이 적어준 것이다.

시간이 좀 걸릴 수 있다. 다 되었다면 설치 가능한 버전들을 아래 명령어로 확인해보자.
```
rbenv install -l
```
최신버전은 호환문제가 생길 수 있으니 나오는 버전들 중 제일 위에서 한두개 아래것을 선택해주자.
나의 경우
```
rbenv install 3.1.4
```
를 해주었다.

역시 설치 후 버전을 확인해본다.
```
rbenv versions
```
```
system
* 3.1.4 (set by /Users/mings/.rbenv/version)
```

이렇게 나올건데 * 표시가 system에 가 있을 수 있다. 이때 아래 명령어를 적어주자.
```
rbenv global 3.1.4
rbenv rehash
```
global 뒤에는 당연히 본인이 설치한 버전을 넣어주어야한다.
다시 버전을 확인해보면 위 사진처럼 * 표시가 아래로 내려가 있을것이다.

# 3. Jekyll 설치
냅다 아래 명령어를 넣어보면
```
gem install bundler 
```

```                                       
Fetching bundler-2.4.19.gem
ERROR:  While executing gem ... (Gem::FilePermissionError)
You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.
```

이렇게 에러가 뜰것이다.

rbenv의 PATH를 추가해주면 된다.
우선 
```
open ~/.zshrc
```
를 입력해주면 파일 하나가 열릴거고 아래에 

```
export PATH={$Home}/.rbenv/bin:$PATH && \
eval "$(rbenv init -)"
```
를 추가해주자. 위 명령어는 터미널에 적는것이 아니다. 방금 열린 텍스트 파일 안에 적어야한다.
저장해주고 터미널로 돌아와서

```
source ~/.zshrc
```
를 해주면 방금 저장한 파일이 적용될것이다.

다시 
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

1 gem installed 보이면 성공!

이제 
```
gem install jekyll
```
적어준다.

만약 에러가 뜬다면 Command Line Tools를 재설치 해보자
```
xcode-select --install
```
하면 되는데 설치되어있다는 에러가 뜰수 있으므로
```
sudo rm -rf /Library/Developer/CommandLineTools
xcode-select --install
```
sudo로 지우고 설치해보자.

완료 후 아까 했던 
```
gem install jekyll
```
해주자.

마지막이다. 자신의 github.io 디렉토리로 이동 후 Gemfile이 있는지 확인해보자 이 파일이 있는 위치에서 
```
bundler install
```
해준다.

이제 끝났고

```
bundle exec jekyll serve
```

해주면 로컬 서버가 열리고 쭉 뜰텐데 

```
Server address: http://127.0.0.1:4000
Server running... press ctrl-c to stop.
```

이런 글이 보일건데 server address 가 보이면 뒤 링크를 주소창에 넣어주자.
본인 블로그가 뜰것인데 로컬에서 보이는 것이기 때문에 지금 당장 여기 보인다고 실제 깃허브 블로그에 반영이 된 것이 아니다.

반영시키려면 git push 해주면 된다.

서버 닫는 방법은 
```
Server running... press ctrl-c to stop.
```
server address 밑에 적혀있다.