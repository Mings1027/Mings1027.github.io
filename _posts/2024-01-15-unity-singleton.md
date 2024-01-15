---
layout: post
title: 유니티 디자인 패턴 Unity Singleton
feature-img : "assets/img/post-img/cat.jpg.avif"
date: January 15th, 2024
tags: [Unity, Admob]
---


흔히 쓰이는 싱글톤일거라고 생각한다. 그러나 내가 생각못했던 부분이 있었다. 만약 게임 시작때가 아닌 진행도중에 새로 만들어질 경우 인스펙터에서 할당한것들은 아무것도 없는채로 만들어지기 때문에 이 싱글톤을 사용하는 클래스의 초기화 부분은 전부 코드로 해주어야한다.

왜 이 간단한 부분을 생각하지 못했냐면 현재 게임에 UIManager와 SoundManager 두개가 싱글톤이고 두개의 씬이 있는데 SoundManager는 두 씬에 모두 쓰여서 게임 시작전에 미리 씬에 올려두었었고 UIManager는 두번째 씬에서만 쓰이기 때문에 다른 리소스들과 함께 어드레서블로 다운받아 쓰고 있었다.

그렇기에 Instance프로퍼티안에 새로 만드는곳에 걸릴 일이 없었던 것이다. 그러다 이 코드를 유심히 보게 되었고 위의 문제가 생겼다. 현재 UIManager는 모든 초기화를 코드로 바꾸었다.

여기서부턴 개인적인 생각인데 싱글톤을 쓰는 이유는 여러 곳에서 쉽게 접근할 수 있고 단 하나의 객체만 존재할 때 쓴다고 한다. 그러면서 DontDestroyOnLoad또한 같이 쓰는 경우가 많던데 왜 이걸 강제 하는지 잘 모르겠다.

만약 씬이 서너개쯤 되는데 두개의 씬에서만 쓰일수도 있지 않나? 그럼 DontDestroy가 되있으면 안쓰는씬에서도 올라와 있게 되는거 아닌가? 아직 정확한 이유를 모르겠다. 일단 강제인것부터 마음에 들지 않아 본인은 이렇게 쓰기로 했다.

```c#
using UnityEngine;


    [DisallowMultipleComponent]
    public abstract class Singleton<T> : MonoBehaviour where T : MonoBehaviour
    {
        private static bool _applicationQuit;
        private static T _instance;

        public static T Instance
        {
            get
            {
                if (_applicationQuit) return null;
                if (!_instance)
                {
                    _instance = (T)FindAnyObjectByType(typeof(T));
                    if (!_instance)
                    {
                        var obj = new GameObject(typeof(T).ToString());
                        _instance = obj.AddComponent<T>();
                    }
                }

                return _instance;
            }
        }

        [SerializeField] private bool dontDestroyOnLoad;

        protected virtual void Awake()
        {
            _applicationQuit = false;
            if (!dontDestroyOnLoad) return;
            var obj = FindObjectsOfType<T>();
            if (obj.Length == 1)
            {
                DontDestroyOnLoad(gameObject);
            }
            else
            {
                Destroy(gameObject);
            }
        }

        private void OnApplicationQuit()
        {
            _applicationQuit = true;
        }
    }
```