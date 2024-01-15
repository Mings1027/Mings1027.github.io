---
layout: post
title: Unity 애드몹 광고 붙이기
feature-img : "assets/img/portfolio/roguedefense.png"
date: January 15th, 2024
tags: [Unity, Admob]
---



## 1. Unity Admob 

우선 유튜브에 잘되어있는 영상이 있었고 본인도 이걸 따라 적용했었습니다.

https://www.youtube.com/watch?v=18qGNDkfdAI

## 2. 전체 코드

```c#
using System;
using CustomEnumControl;
using GameControl;
using GoogleMobileAds.Api;
using ManagerControl;
using UnityEngine;

namespace GoogleMobileAdmob
{
    public class AdmobManager : Singleton<AdmobManager>
    {
        private static RewardedAd _rewardedAd;
        private static bool _isTestMode;
        private const string testRewardedId = "ca-app-pub-3940256099942544/712485313";
#if UNITY_IPHONE
        private static string RewardedId = "";
#elif UNITY_ANDROID
        private static string RewardedId = "";
#endif

        private void Start()
        {
            MobileAds.RaiseAdEventsOnUnityMainThread = true;
            MobileAds.Initialize(initStatus => { print("Ads Initialised !!!!!!!!!!!!!!!!!"); });
        }

#region Rewarded

        public static void LoadRewardedAd()
        {
            SoundManager.Instance.MuteBGM(true);
            if (_rewardedAd != null)
            {
                _rewardedAd.Destroy();
                _rewardedAd = null;
            }

            var adRequest = new AdRequest();
            adRequest.Keywords.Add("unity-admob-sample");

            RewardedAd.Load(_isTestMode ? testRewardedId : RewardedId, adRequest, (ad, error) =>
            {
                if (error != null || ad == null)
                {
                    print("Rewarded failed to load" + error);
                    return;
                }

                print("Rewarded ad loaded !!!!!!!!!!");
                _rewardedAd = ad;
                RewardedAdEvents(_rewardedAd);
            });
        }

        public static void ShowRewardedAd()
        {
            if (_rewardedAd != null && _rewardedAd.CanShowAd())
            {
                _rewardedAd.Show(_ =>
                {
                    print("Give reward to player~~~~~~~~~~~~~~");
                });
            }
            else
            {
                print("Rewarded ad not ready");
            }
        }

        public static void RewardedAdEvents(RewardedAd ad)
        {
            // Raised when the ad is estimated to have earned money.
            ad.OnAdPaid += (AdValue adValue) =>
            {
                Debug.Log($"Rewarded ad paid {adValue.Value} {adValue.CurrencyCode}.");
            };
            // Raised when an impression is recorded for an ad.
            ad.OnAdImpressionRecorded += () => { Debug.Log("Rewarded ad recorded an impression."); };
            // Raised when a click is recorded for an ad.
            ad.OnAdClicked += () => { Debug.Log("Rewarded ad was clicked."); };
            // Raised when an ad opened full screen content.
            ad.OnAdFullScreenContentOpened += () => { Debug.Log("Rewarded ad full screen content opened."); };
            // Raised when the ad closed full screen content.
            ad.OnAdFullScreenContentClosed += () =>
            {
                Debug.Log("Rewarded ad full screen content closed.");
                LoadRewardedAd();
                SoundManager.Instance.MuteBGM(false);
            };
            // Raised when the ad failed to open full screen content.
            ad.OnAdFullScreenContentFailed += (AdError error) =>
            {
                Debug.LogError("Rewarded ad failed to open full screen content with error : " + error);
            };
        }

#endregion
    }
}
```

코드를 보면 알겠지만 리워드 광고만 있습니다 방식이 크게 다르지 않고 현재 제가 쓰는것이기에 그대로 올렸습니다.

RewardedAdEvents() 함수는 
[이 링크](https://developers.google.com/admob/unity/rewarded?hl=ko#ios)로 들어가면 설명이 있습니다.

코드는 복붙해서 광고 아이디만 맞는걸로 채워서 그대로 사용해도 됩니다.

아래는 테스트 id 입니다.

iOS
배너광고  ca-app-pub-3940256099942544/2934735716
전면광고 ca-app-pub-3940256099942544/4411468910
보상형 동영상 광고 ca-app-pub-3940256099942544/1712485313
네이티브 광고 고급형 ca-app-pub-3940256099942544/3986624511

Android
배너광고  ca-app-pub-3940256099942544/6300978111
전면광고 ca-app-pub-3940256099942544/1033173712
보상형 동영상 광고 ca-app-pub-3940256099942544/5224354917
네이티브 광고 고급형 ca-app-pub-3940256099942544/2247696110

## 3. 주의사항

본인은 아직 겪지 않았지만 빌드 테스트 때 실제 admob의 광고 id를 그대로 쓰면 블락당할 위험이 있고 그런 사례가 많다고 합니다. 테스트 때는 테스트 id만 사용하시기 바랍니다. 

혹시나 위 코드를 그대로 쓸 때 앱 심사에 올리는 버전이 아니라면 꼭 isTestMode 변수를 켜놓고 테스트 id가 맞는지 확인 후 쓰시기 바랍니다.