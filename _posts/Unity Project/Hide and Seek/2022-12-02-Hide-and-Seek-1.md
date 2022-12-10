---
title: "Hide and Seek (1) - 기본 구성"
excerpt: "Hide and Seek - (1)"

categories:
  - Hide and Seek
tags:
  - Unity Project
  - Hide and Seek

toc: true
toc_sticky: true

date: 2022-12-02
last_modified_at: 2022-12-03
---

## 1. 기본 맵

![image](https://user-images.githubusercontent.com/85764911/205439356-5e22125a-1de7-44ab-a9b5-0dde6b9292a4.png)

외부에서 에셋을 가져와서 간단하게 평지를 제작 (후에 scale up)

## 2. 기본 플레이어

![image](https://user-images.githubusercontent.com/85764911/205439649-74c39d96-76c3-4fdd-bf1a-6b4f757804b2.png)

또한 외부 에셋으로 플레이어 캐릭터를 생성

## 3. 플레이어 WASD moving

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
	[SerializeField]
	private float _walkSpeed;
	private Rigidbody _rigidbody;

	private void Awake()
	{
		_rigidbody = GetComponent<Rigidbody>();
	}

	private void FixedUpdate()
	{
		Move();
	}

	private void Move()
	{
		float _moveX = Input.GetAxisRaw("Horizontal");
		float _moveZ = Input.GetAxisRaw("Vertical");

		Vector3 _moveHorizontal = transform.right * _moveX;
		Vector3 _moveVertical = transform.forward * _moveZ;

		Vector3 _velocity = (_moveHorizontal + _moveVertical).normalized * _walkSpeed;

		_rigidbody.MovePosition(gameObject.transform.position + _velocity * Time.deltaTime);
	}
}
```

널리 알려진 이동 스크립트

## 4. 플레이어 마우스 rotating

이제 위 스크립트에 추가로 마우스의 이동에 따라 플레이어가 회전하도록 추가할 것이다.

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
	...

	/* Variables for rotating */
	[SerializeField]
	private float _mouseSensitivity;
	[SerializeField]
	private GameObject _neck;
	[SerializeField]
	private GameObject _body;

	...

	private void Update()
	{
		Rotate();
	}

	...

	private void Rotate()
	{
		float _rotateSpeedX = Input.GetAxisRaw("Mouse X") * _mouseSensitivity * Time.deltaTime;

		_neck.transform.Rotate(new Vector3(0f, _rotateSpeedX, 0f));
		if (Mathf.Abs(_neck.transform.rotation.eulerAngles.y - _body.transform.rotation.eulerAngles.y) > 35)
		{
			_body.transform.Rotate(new Vector3(-_rotateSpeedX, 0f, 0f));
			_neck.transform.Rotate(new Vector3(0f, -_rotateSpeedX, 0f));
		}
	}
}
```

위에서 생성한 캐릭터는 **Rigged**된 3D Model이다.


내가 의도한 캐릭터 회전은 마인크래프트와 비슷한 방식으로, 마우스가 회전하면 우선 머리만 그에 따라 회전하도록 하고, 만약 머리가 자신의 몸에 비해 과도하게 회전됐다면 그걸 감지해 몸도 함께 회전되도록 하는 방식이다.

