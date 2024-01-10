---
layout: post
title: Rogue Defense
feature-img: "assets/img/portfolio/roguedefenseingame.png"
img: "assets/img/portfolio/roguedefense.png"
date: January 2024
---

![image]({{ page.img | relative_url }})

현재 출시 예정인 모작 프로젝트 입니다.

## 참고한 게임
[Rogue Tower](https://store.steampowered.com/app/1843760/Rogue_Tower/)  
[Kindom Rush](https://store.steampowered.com/app/246420/Kingdom_Rush___Tower_Defense/)

## 주요 구현 사항

### 절차적 맵 생성

[이 링크](https://github.com/Mings1027/UnityGame/blob/main/TowerDefense/Assets/Scripts/MapControl/MapManager.cs)로 해당 코드를 볼 수 있으며 
각 타일은 [MapData](https://github.com/Mings1027/UnityGame/blob/main/TowerDefense/Assets/Scripts/MapControl/MapData.cs) 스크립트를 가지고 있습니다.

## 맵이 생성되는 과정
{% include aligner.html images="portfolio/GenerateMap.gif" column=1 %}

1. 
<span style="font-size: 23px; color: white">
초기 타일의 중심에서 사방으로 길이 난 방향에 새로운 타일이 생성될 수 있도록 버튼을 설치합니다.

2.
<span style="font-size: 23px; color: white">
버튼을 누르면 그 위치에 새 타일이 만들어지고 새 타일의 중심에서 사방으로 이웃한 타일이 있는지 확인합니다.

3.
<span style="font-size: 23px; color: white">
이웃타일이 있고 이웃타일의 길 방향이 새 타일을 향하는지 확인하고 향한다면 새 타일과 이웃타일을 이어줍니다.

4.
<span style="font-size: 23px; color: white">
이웃타일이 없다면 정해진 확률에 의해 그 방향으로 길이 만들어집니다.

5.
<span style="font-size: 23px; color: white">
만약 사방에 다 이웃 타일이 있는데 이웃타일들의 길 중 단 하나만 새 타일을 향하고 있다면 막힌 길 이므로 단방향 타일을 만들고 그 이웃타일과 이어줍니다.

```c#

    private void CheckNeighborMap()
    {
        var checkDirCount = _checkDirection.Length;

        for (var i = 0; i < checkDirCount; i++)
        {
            var ray = new Ray(_newMapPos, _checkDirection[i]);

            if (Physics.SphereCast(ray, 2, out var hit, mapSize, groundLayer))
            {
                if (hit.collider.TryGetComponent(out MapData mapData))
                {
                    _neighborMapArray[i] = mapData;
                }
            }
            else
            {
                _isEmptyMapArray[i] = true;
            }
        }
    }

    private void CheckConnectedDirection()
    {
        var neighborMapCount = _neighborMapArray.Length;

        for (var i = 0; i < neighborMapCount; i++)
        {
            if (_isEmptyMapArray[i]) continue;

            var neighborPos = _neighborMapArray[i].transform.position;
            var neighborToNewMapDir = (_newMapPos - neighborPos).normalized;
            var neighborWayPoints = _neighborMapArray[i].wayPointList;
            var neighborWayPointCount = neighborWayPoints.Count;

            for (var j = 0; j < neighborWayPointCount; j++)
            {
                var dir = (neighborWayPoints[j] - neighborPos).normalized;

                if (neighborToNewMapDir != dir) continue;
                _isConnectedArray[i] = true;
            }
        }
    }

    private void AddRandomDirection()
    {
        _tileConnectionIndex = null;
        _emptyTileIndex = null;
        var emptyMapArrayLength = _isEmptyMapArray.Length;
        var isConnected = false;
        var tempProbability = _wayPointsHashSet.Count == 1 ? 100 : roadProbability;

        for (var i = 0; i < emptyMapArrayLength; i++)
        {
            if (_isEmptyMapArray[i])
            {
                _emptyTileIndex += i;
                var ran = Random.Range(0, 101);

                if (ran <= tempProbability)
                {
                    isConnected = true;
                    _tileConnectionIndex += i;
                }
            }
            else
            {
                if (_isConnectedArray[i])
                {
                    _tileConnectionIndex += i;
                }
            }
        }

        if (_emptyTileIndex == null) return;
        //사방이 막히지 않았을 때 아래를 실행

        // 하나만 연결된 경우 랜덤으로 길 하나를 뚫어줌
        if (_tileConnectionIndex is {Length: 1})
        {
            var tempString = new string(_emptyTileIndex);
            _tileConnectionIndex += tempString[Random.Range(0, tempString.Length)];
        }

        //타일이 없는곳에 연결이 한번도 안되었을때 혹은 연결된 인덱스와과 타일이 없는곳의 인덱스 길이 같으면서 정반대일 때 닫힐 수 있으므로 길 하나를 뚫어줌
        if (!isConnected || _tileConnectionIndex != null && _tileConnectionIndex.Length == _emptyTileIndex.Length && _tileConnectionIndex[0] != _emptyTileIndex[0])
        {
            var ranIndex = Random.Range(0, _emptyTileIndex.Length);
            _tileConnectionIndex += _emptyTileIndex[ranIndex];
            _tileConnectionIndex = new string(_tileConnectionIndex?.Distinct().ToArray());
        }
    }
```
아래 두 변수로 타일의 모양이 결정되기 때문에 가장 중요한 변수들이라고 말할 수 있습니다.  <br />

<span style="font-size: 23px;">
_tileConnectionIndex  :  새 타일의 중심에서 길이 이어진 곳의 인덱스를 저장합니다.    
_emptyTileIndex  :  새 타일의 중심에서 이웃타일이 없는 곳의 인덱스를 저장합니다.  
- 인덱스는 0 1 2 3 중에 선택되며 순서대로 back, forward, left, right 방향을 의미합니다.

## 예외 처리
위 과정으로 타일들이 이어지다 보면 예외가 발생할 수 있기 때문에 추가로 길을 뚫는 과정이 필요합니다.  
설명을 적기 전에 두개의 변수에 대한 설명을 먼저 적겠습니다.

### 1.단방향 타일로 인해 닫힌 맵이 만들어집니다.  

{% include aligner.html images="portfolio/CloseMap1.png" %}

단방향 타일이 필요한 이유는 아래 왼쪽 사진처럼 맵이 만들어졌을 때 사방이 다 막혀있는데 길이 이어지지 않을 수 있습니다.  
그래서 오른쪽사진처럼 한쪽으로 이어진 타일이 필요하고 여기에 포탈을 만들어 몬스터가 포탈을 타고 넘어오는 설정을 만들 수 있게 됩니다.
{% include aligner.html images="portfolio/BeforePortalMap.png,portfolio/AfterPortalMap.png" %}

해결방법은 간단합니다.  사방이 막히지 않았는데 길이 하나만 연결된다면 추가로 길을 하나 더 만들어주면 됩니다.
```c#
// 하나만 연결된 경우 랜덤으로 길 하나를 뚫어줌
if (_tileConnectionIndex is {Length: 1})
{
    var tempString = new string(_emptyTileIndex);
    _tileConnectionIndex += tempString[Random.Range(0, tempString.Length)];
}
```

### 2.타일의 시작과 끝이 만나 닫힌 맵이 만들어집니다.

{% include aligner.html images="portfolio/CloseMap2.png" %}

아래는 이해를 돕기위해 임의로 맵을 제작하였습니다. 빨간 점이 처음 만들어진 타일의 위치입니다.  
왼쪽사진처럼 만들어질 수 있는 타일이 단 하나남았다면 그리고 그 타일이 오른쪽 사진처럼 만들어진다면 맵이 닫혀버립니다.
{% include aligner.html images="portfolio/TestMap.png,portfolio/TestMap1.png" %}
그렇기 때문에 아래의 과정이 필요하게 됩니다.  
```c#
//타일이 없는곳에 연결이 한번도 안되었을때 혹은 연결된 인덱스와 타일이 없는곳의 인덱스 길이가 같으면서 정반대일 때 닫힐 수 있으므로 길 하나를 뚫어줌
if (!isConnected || _tileConnectionIndex != null && _tileConnectionIndex.Length == _emptyTileIndex.Length)
{
    var ranIndex = Random.Range(0, _emptyTileIndex.Length);
    _tileConnectionIndex += _emptyTileIndex[ranIndex];
    _tileConnectionIndex = new string(_tileConnectionIndex?.Distinct().ToArray());
}
```
주석을 보면 설명이 적혀있는데 첫번째 조건 isConnected는 타일이 없는 곳에 길이 이어졌을 때 true가 됩니다.
|| 뒤의 조건은 tileConnectionIndex와 emptyTileIndex의 길이가 같을때. 즉, 위 사진처럼 새 타일의 중심에서 타일이 있는 곳이 두군데고, 타일이 있는곳만 길이 이어진다면 두 변수는 길이가 같아집니다.
아래 표로 확인해보겠습니다.

| tileConnectionIndex | emptyTileIndex |
|---------------------|----------------|
|         0 1         |       2 3      |
|         0 2         |       1 3      |
|         0 3         |       1 2      |
|         1 2         |       0 3      |
|         1 3         |       0 2      |
|         2 3         |       0 1      |

결국 두개씩 고르는 경우의 수 입니다. 이런경우들이 맵을 닫히게 할 가능성이 있는 경우들 입니다.  
그래서 이런경우 길을 하나 더 만들어줍니다.

이 때 길이가 둘 다 3인 경우가 생길 수 있습니다. 그러나 앞의 조건 isConnected로 걸러지기 때문에 길이가 2인경우만 고려하면 됩니다.

<script src="https://utteranc.es/client.js"
        repo="Mings1027/Mings1027.github.io"
        issue-term="pathname"
        label="utterances"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>