---
title: "Hide and Seek (1) - 멀티플레이를 위한 Netcode 설치"
excerpt: "Hide and Seek - (1)"

categories:
  - Hide and Seek
tags:
  - Unity Project
  - Hide and Seek

toc: true
toc_sticky: true

date: 2022-12-02
last_modified_at: 2022-12-02
---

## 1. Netcode 설치

1. 메뉴바에서, **Window** -> **Package Manager**를 클릭
2. 왼쪽 상단의 **+** 버튼을 누르고 **Add package by name**을 클릭
3. 나타나는 팝업 창에 `com.unity.netcode.gameobjects`을 검색
4. 오른쪽 하단의 **Install**을 클릭

## 2. 멀티플레이 테스트

**Network Manager** 생성

1. **Hierarchy** 탭에서 **Create Empty**로 빈 `GameObject` 생성
2. `GameObject`의 이름을 `NetworkManager`로 변경
3. `NetworkManager` 오브젝트에 `Network Manager` 컴포넌트 추가
4. `Network Manager` 컴포넌트 탭의 `Network Transport`의 필드를 `Unity Transport`로 설정

<br>

멀티플레이를 위한 **Player** 생성

1. **Hierarchy** 탭에서 **3D Object** -> **Capsule** 생성, 이름을 `Player`로 변경
2. `Player` 오브젝트에 `Network Object` 컴포넌트 추가
3. `Player` 오브젝트를 **Prefab**으로 추가
4. `NetworkManager` 오브젝트의 `Network Manager` 컴포넌트의 `Player Prefab` 필드에 `Player` 프리팹 드래그&드롭
5. **Scene**의 `Player` 오브젝트 제거

<br>

현재 씬을 **Build**에 포함시키기

1. 메뉴바에서, **File** -> **Build Settings** 클릭
2. **Add Open Scenes** 클릭 (현재 씬을 **Build**에 추가)

<br>

**Build**에 도움을 주는 **Command Line Helper** 생성

1. `NetworkCommandLine`의 이름으로 **C# Script** 생성
2. `NetworkManager` 오브젝트의 자식으로 빈 `GameObject` 생성, 이름을 `NetworkCommandLine`으로 변경
3. `NetworkCommandLine`에 `NetworkCommandLine.cs` 컴포넌트 추가
4. `NetworkCommandLine.cs`를 다음과 같이 수정

```cs
using System.Collections.Generic;
using Unity.Netcode;
using UnityEngine;

public class NetworkCommandLine : MonoBehaviour
{
  private NetworkManager netManager;

  private void Start()
  {
    netManager = GetComponentInParent<NetworkManager>();

    if (Application.isEditor) return;

    var args = GetCommandlineArgs();

    if (args.TryGetValue("-mode", out string mode))
    {
      switch (mode)
      {
        case "server":
          netManager.StartServer();
          break;
        case "host":
          netManager.StartHost();
          break;
        case "client":
          netManager.StartClient();
          break;
      }
    }
  }

  private Dictionary<string, string> GetCommandlineArgs()
  {
    Dictionary<string, string> argDictionary = new Dictionary<string, string>();

    var args = System.Environment.GetCommandLineArgs();

    for (int i = 0; i < args.Length; i++)
    {
      var arg = args[i].ToLower();
      if (arg.StartsWith("-"))
      {
        var value = i < args.Length - 1 ? args[i + 1].ToLower() : null;
        value = (value?.StartsWith("-") ?? false) ? null : value;

        argDictionary.Add(arg, value);
      }
    }
    return argDictionary;
  }
}
```
위 코드는 **Command Line Interface**에서 입력되는 명령어에 따라게임을 각각 **Server**, **Host**, **Client**로 실행할지 구분해주는 기능을 한다.


<br>

**Command line helper** 테스트하기

1. **File** -> **Build and Run** 클릭
2. 프로젝트 폴더에 **Build** 폴더를 생성하고 그 안에 실행파일 저장
3. **CLI**에서 다음의 명령을 입력

    - `<path to project>/Build/<program name>.exe - mode { server | host | client }`

이제 서버를 통해 게임을 실행시키고 또 다른 **CLI**를 통해 클라이언트로 게임을 실행시키면 두 게임 화면 모두에 `Player` 프리팹이 나타나는 것을 볼 수 있다.