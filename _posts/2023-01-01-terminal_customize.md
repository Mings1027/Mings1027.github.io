---
title: "masOS 터미널 꾸미는 방법"
excerpt: "맥에서 터미널 꾸미는 방법을 알아보자"

categories: Blog
tags:
    - [blog]

toc: true
toc_sticky: true
author_profile: false
sidebar:
    nav: "docs"

date: 2023-01-01
last_modified_at: 2023-01-01
---







# 1. oh-my-zsh 설치하기

```
sh -c "$(curl -fsSL <https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh>)"
```
터미널에 붙여넣어서 설치하자

# 2. 테마 변경하기

테마 변경은 ~/.zshrc 파일을 열면 ZSH_THEME= “  “ 부분이 있을거다. 이 부분에 원하는 테마를 넣어준다
zshrc 파일을 열려면 터미널에 다음과 같이 적어준다
```
open ~/.zshrc
```
나의 경우 맥이기 때문에 사과모양이 좋아서 
```
ZSH_THEME= “apple“
```
이렇게 해주었다.

# 3. 유용한 플러그인 추천

## 1. syntax-highlighting 
```
brew install zsh-syntax-highlighting
```
```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
글자를 하이라이팅 해준다 맞는 단어는 초록색 없거나 틀린 단어는 빨간색으로 스펠링 틀린걸 바로 체크할 수 있어서 유용하게 사용하고 있다.

![terminal2](https://user-images.githubusercontent.com/100500113/210172459-31b75c1f-7283-4cea-b033-0b7af41c9be0.png)

## 2. zsh-autosuggestions
```
brew install zsh-autosuggestions
```
```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
내가 자주 사용하는 명령어나 파일 이름 등을 제안해준다. 

![terminal1](https://user-images.githubusercontent.com/100500113/210172440-10954208-531f-43ec-872a-6f932b3eeb84.png)

## 3. zsh-autojump
```
brew install autojump
```
```
git clone git://github.com/wting/autojump.git
```
사용법은 간단하다. j + 디렉토리명

![terminal3](https://user-images.githubusercontent.com/100500113/210172501-1d3775a9-e138-486c-b82a-e097b1a64028.png)

## 4. 적용

open ~/.zshrc 로 파일 열어서 밑에 theme과 plugins 설정해준다.

```
ZSH_THEME="apple"
plugins=(git
zsh-autosuggestions
zsh-syntax-highlighting
autojump)
```