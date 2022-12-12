---
title: "Hide and Seek (3) - 멀티플레이 플레이어 재구성"
excerpt: "Hide and Seek - (3)"

categories:
  - Hide and Seek
tags:
  - Unity Project
  - Hide and Seek

toc: true
toc_sticky: true

date: 2022-12-12
last_modified_at: 2022-12-12
---

## 1. 기본 구성

플레이어 **Prefab**의 자식인 **Camera** 오브젝트 삭제 <br>
**Cinemachine**을 이용해 카메라를 재구성한다.

(카메라가 플레이어를 1인칭으로 따라가게 하는 것에 어려움이 있었음)

<br>

## 2. 시네머신 카메라 구성

일반 **Camera** 오브젝트를 생성 (Brain Camera로 다른 Virtual Camera를 조종) <br>
**Camera** 오브젝트의 컴포넌트로 **Cinemachine Virtual Camera** 추가

1인칭 카메라로 설정하기 위해 다음을 변경
![image](https://user-images.githubusercontent.com/85764911/207091827-a4554306-63dc-4d14-9dba-7b6178e5ad1f.png)

- **Body**
  - **Transposer**로 변경
  - **Binding Mode**를 **Lock To Target With World Up**으로 변경
  - **Follow Offset**을 (0, 1, 0)으로 변경 (y가 1인 이유는 플레이어의 눈높이 설정)
  - 각 **Damping**을 0으로 설정
- **Aim**
  - **Same As Follow Target**으로 변경

<br>

## 3. CameraFollow 스크립트

```cs
using System.Collections;
using System.Collections.Generic;
using Cinemachine;
using Photon.Pun;
using UnityEngine;

public class CameraFollow : MonoBehaviourPun
{
	private void Start()
	{
		if (photonView.IsMine)
		{
			CinemachineVirtualCamera followCam =
				FindObjectOfType<CinemachineVirtualCamera>();

			followCam.Follow = transform;
			followCam.LookAt = transform;
		}
	}
}
```
만약 플레이어가 로컬 플레이어라면 씬의 **Virtual Camera**를 불러서 자신의 1인칭 시점을 촬영하라고 시키는 스크립트