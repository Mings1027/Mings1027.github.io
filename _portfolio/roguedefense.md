---
layout: post
title: Rogue Defense
feature-img: "assets/img/portfolio/roguedefenseingame.png"
img: "assets/img/portfolio/roguedefense.png"
date: January 2024
---

![image]({{ page.img | relative_url }})

현재 출시 예정인 모작 프로젝트 입니다.
참고한 게임은 [Rogue Tower](https://store.steampowered.com/app/1843760/Rogue_Tower/) 스팀게임입니다. 

## 주요 구현 사항

### 절차적 맵 생성

[이 링크](https://github.com/Mings1027/UnityGame/blob/main/TowerDefense/Assets/Scripts/MapControl/MapManager.cs)로 해당 코드를 볼 수 있으며 각 타일은 [MapData](https://github.com/Mings1027/UnityGame/blob/main/TowerDefense/Assets/Scripts/MapControl/MapData.cs) 스크립트를 가지고 있습니다.

## 맵이 생성되는 과정

#### 1.
초기 타일의 중심에서 길이 난 방향에 새로운 타일이 생성되는 방식입니다.

#### 2.
새로운 타일은 그 타일의 중심에서 사방으로 이웃한 타일이 있는지 먼저 확인합니다.

#### 3.
이웃타일이 새로운 타일 방향으로 길이 나있는지 확인합니다.

#### 4.
새 타일은 이웃타일 방향으로 길을 잇고, 이웃이 없는 방향의 길은 확률로 만들어집니다.

#### 5.
다음 gif가 타일이 하나씩 만들어지는 과정입니다.
{% include aligner.html images="portfolio/mapgeneration.gif" column=1 %}

