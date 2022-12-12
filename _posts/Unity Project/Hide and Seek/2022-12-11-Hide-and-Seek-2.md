---
title: "Hide and Seek (2) - 멀티플레이 로비"
excerpt: "Hide and Seek - (2)"

categories:
  - Hide and Seek
tags:
  - Unity Project
  - Hide and Seek

toc: true
toc_sticky: true

date: 2022-12-11
last_modified_at: 2022-12-11
---


## 1. 기본 구성

![image](https://user-images.githubusercontent.com/85764911/206899574-d3ce5856-08e0-42e2-b9b5-1cd866412012.png)
간단한 로비 화면을 구성한다.

<br>

## 2. 로비 스크립트

```cs
using Photon.Pun;
using Photon.Realtime;
using UnityEngine;
using UnityEngine.UI;
using TMPro;

public class LobbyManager : MonoBehaviourPunCallbacks
{
	private string _gameVersion = "1";

	[SerializeField]
	private TextMeshProUGUI _connectionInfoText;
	[SerializeField]
	private Button _joinButton;

	private void Start()
	{
		PhotonNetwork.GameVersion = _gameVersion;
		PhotonNetwork.ConnectUsingSettings();

		_joinButton.interactable = false;
		_connectionInfoText.text = "마스터 서버에 접속 중...";
	}

	public override void OnConnectedToMaster()
	{
		_joinButton.interactable = true;
		_connectionInfoText.text = "온라인 : 마스터 서버와 연결됨";
	}

	public override void OnDisconnected(DisconnectCause cause)
	{
		_joinButton.interactable = false;
		_connectionInfoText.text = "오프라인 : 마스터 서버와 연결되지 않음\n접속 재시도 중...";

		PhotonNetwork.ConnectUsingSettings();
	}

	public void Connect()
	{
		_joinButton.interactable = false;

		if (PhotonNetwork.IsConnected)
		{
			_connectionInfoText.text = "룸에 접속...";
			PhotonNetwork.JoinRandomRoom();
		}
		else
		{
			_connectionInfoText.text = "오프라인 : 마스터 서버와 연결되지 않음\n접속 재시도 중...";
			PhotonNetwork.ConnectUsingSettings();
		}
	}

	public override void OnJoinRandomFailed(short returnCode, string message)
	{
		_connectionInfoText.text = "빈 방이 없음, 새로운 방 생성...";
		PhotonNetwork.CreateRoom(null, new RoomOptions { MaxPlayers = 4 });
	}

	public override void OnJoinedRoom()
	{
		_connectionInfoText.text = "방 참가 성공";
		PhotonNetwork.LoadLevel("MainLobby");
	}
}
```
**Photon**을 이용한 멀티플레이 로비

### Start()
```cs
private void Start()
{
	PhotonNetwork.GameVersion = _gameVersion;
	PhotonNetwork.ConnectUsingSettings();
	
	_joinButton.interactable = false;
	_connectionInfoText.text = "마스터 서버에 접속 중...";
}
```
`Start()` 메서드에서는 게임의 버전을 설정하고, 해당 설정으로 마스터 서버에 접속을 시도한다. <br>
또한 `_joinButton`과 `_connectionInfoText`를 설정한다.

<br>

### OnConnectedToMaster()
```cs
public override void OnConnectedToMaster()
{
	_joinButton.interactable = true;
	_connectionInfoText.text = "온라인 : 마스터 서버와 연결됨";
}
```
마스터 서버에 접속 성공하면 자동으로 실행된다. <br>
`Start()` 메서드에서 `PhotonNetwork.ConnectUsingSettings();`으로 마스터 서버에 정상적으로 접속되면 실행된다.

접속 성공 시 `_joinButton`을 활성화시키고 `_connectionInfoText`의 텍스트를 바꾼다.

<br>

### OnDisconnected()
```cs
public override void OnDisconnected(DisconnectCause cause)
{
	_joinButton.interactable = false;
	_connectionInfoText.text = "오프라인 : 마스터 서버와 연결되지 않음\n접속 재시도 중...";

	PhotonNetwork.ConnectUsingSettings();
}
```
마스터 서버에 접속 실패했거나 접속된 상태에서 어떠한 이유로 접속이 끊겼을 경우 실행된다. <br>
접속 끊김의 원인에 대한 정보가 `DisconnectCause` 타입으로 메서드에 자동으로 전달된다.

`_joinButton`, `_connectionInfoText`를 설정하고 마스터 서버로의 재접속을 시도해야 한다.

<br>

### Connect()
```cs
public void Connect()
{
	_joinButton.interactable = false;

	if (PhotonNetwork.IsConnected)
	{
		_connectionInfoText.text = "룸에 접속...";
		PhotonNetwork.JoinRandomRoom();
	}
	else
	{
		_connectionInfoText.text = "오프라인 : 마스터 서버와 연결되지 않음\n접속 재시도 중...";
		PhotonNetwork.ConnectUsingSettings();
	}
}
```
씬의 **Join Button**을 클릭했을 때 실행된다. <br>
`Connect()` 메서드는 매치메이킹 서버(마스터 서버)를 통해 빈 무작위 룸에 접속을 시도한다.

`_joinButton`을 비활성화시켜서 중복 접속을 방지하고, 마스터 서버에 접속 중인 경우와 그렇지 않은 경우를 나누어서 버튼과 텍스트의 상태를 설정한다. <br>
만약 접속 중이 아니라면 마스터 서버로 재접속 될 수 있게 코드를 추가한다.

<br>

### OnJoinRandomFailed()
```cs
public override void OnJoinRandomFailed(short returnCode, string message)
{
	_connectionInfoText.text = "빈 방이 없음, 새로운 방 생성...";
	PhotonNetwork.CreateRoom(null, new RoomOptions { MaxPlayers = 4 });
}
```
랜덤 룸 접속에 실패한 경우 실행된다. (마스터 서버와의 연결이 끊긴 것이 아님) <br>
빈 자리가 있는 랜덤 룸이 없을 경우에 실행되므로, 새로운 룸을 만들어서 접속할 수 있도록 처리를 해야 한다.

<br>

### OnJoinedRoom()
```cs
public override void OnJoinedRoom()
{
	_connectionInfoText.text = "방 참가 성공";
	PhotonNetwork.LoadLevel("MainLobby");
}
```
룸 참가에 성공한 경우 실행된다. <br>
자신이 룸을 직접 생성하고 참가한 경우에도 실행된다.

`PhotonNetwork.LoadLevel()` 메서드는 룸의 플레이어들이 함께 새로운 무대로 이동하는 메서드이다. <br>
방장 플레이어가 이 메서드를 실행하게 되면 다른 플레이어의 컴퓨터에서도 `PhotonNetwork.LoadLevel()` 메서드가 실행되게 된다. <br>
도중에 참가한 플레이어들을 따로 처리할 필요가 없어서 편리하다.

<br>

## 플레이어 캐릭터 네트워킹

**Player** 오브젝트에 컴포넌트를 추가한다.

<br>

### Photon View
![image](https://user-images.githubusercontent.com/85764911/206907789-2da81fdd-8915-4d7c-a1d8-0bc353069284.png)

네트워크를 통해서 동기화되는 모든 게임 오브젝트는 **Photon View** 컴포넌트를 가져야 한다.

게임 오브젝트에 네트워크상에서 구별되도록 식별자인 **View ID**를 부여한다. <br>
또한 **Observed Components** 리스트에 등록된 컴포넌트들을 관측하고, 다른 클라이언트에 전달할 수 있다. <br>

즉, **Photon View** 컴포넌트는 게임 오브젝트가 로컬과 리모트 구분이 가능해지도록 하며, 다른 클라이언트에 존재하는 '나'에게 관측된 수치를 전달하고 동기화할 수 있다.

**Observed Components** 리스트에 넣을 수 있는 컴포넌트는 `IPunObservable` 인터페이스를 상속한 컴포넌트여야 한다.

**Synchronization**
- 값이 동기화되는 방식을 결정한다.
  - Off : 동기화 X
  - Reliable Delta Compressed : 상대방이 최근에 수신한 값과 동일한 값은 송신하지 않음
  - Unreliable : 패킷의 수신 여부를 검사하지 않고 지속적으로 송신
  - Unreliable On Change : Unreliable과 동일, 값의 변화가 감지될 때만 송신

<br>

### Photon Transform View
![image](https://user-images.githubusercontent.com/85764911/206907940-ede5fb53-86f0-4731-9b42-a931d201071e.png)

자신의 게임 오브젝트의 **Transform** 컴포넌트 값의 변화르 측정하고, **Photon View** 컴포넌트를 통해 동기화한다. <br>
위의 사진에서는 **Position**과 **Rotation**이 동기화되도록 설정되어 있다.

**Photon Transform View** 컴포넌트는 **Photon View** 컴포넌트 없이 동작할 수 없다.

<br>

### PlayerController 수정

```cs
private void Update()
{
	if (!photonView.IsMine)
	{
		return;
	}

	...
}

private void FixedUpdate()
{
	if (!photonView.IsMine)
	{
		return;
	}

	...
}
```

현재 게임 오브젝트가 로컬 게임 오브젝트일 경우에만 `Update()`로 플레이어가 움직일 수 있도록 코드를 수정한다.

