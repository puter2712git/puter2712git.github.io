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
last_modified_at: 2022-12-11
---

## 1. 기본 구성
![image](https://user-images.githubusercontent.com/85764911/206886086-421af029-cd37-4c4e-836f-8c8cc0d6f799.png)
**Plane**으로 맵과 **Capsule**로 플레이어 생성

<br>

## 2. 플레이어 Input 감지

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
	private float _horizontalKeyInput;
	private float _verticalKeyInput;
	private float _xMouseInput;
	private float _yMouseInput;

	[SerializeField]
	private float _walkSpeed;

	private void Update()
	{
		DetectInput();
	}

	private void DetectInput()
	{
		_horizontalKeyInput = Input.GetAxisRaw("Horizontal");
		_verticalKeyInput = Input.GetAxisRaw("Vertical");

		_xMouseInput = Input.GetAxisRaw("Mouse X");
		_yMouseInput = Input.GetAxisRaw("Mouse Y");
	}
}
```
매 프레임마다 **Input**을 감지하는 코드

<br>

## 3. 플레이어 WASD 이동

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
	private float _horizontalKeyInput;
	private float _verticalKeyInput;
	private float _xMouseInput;
	private float _yMouseInput;

	[SerializeField]
	private float _walkSpeed;
	private Rigidbody _rigidbody;

	private void Awake()
	{
		_rigidbody = GetComponent<Rigidbody>();
	}

	private void Update()
	{
		DetectInput();
	}

	private void FixedUpdate()
	{
		Move();
	}

	private void DetectInput()
	{
		_horizontalKeyInput = Input.GetAxisRaw("Horizontal");
		_verticalKeyInput = Input.GetAxisRaw("Vertical");

		_xMouseInput = Input.GetAxisRaw("Mouse X");
		_yMouseInput = Input.GetAxisRaw("Mouse Y");
	}

	private void Move()
	{
		Vector3 moveHorizontal = transform.right * _horizontalKeyInput;
		Vector3 moveVertical = transform.forward * _verticalKeyInput;

		Vector3 velocity = (moveHorizontal + moveVertical).normalized * _walkSpeed;

		_rigidbody.MovePosition(transform.position + velocity * Time.deltaTime);
	}
}
```
**Input** 값을 받아서 `Rigidbody` 컴포넌트로 위치를 변경하는 함수를 만들고, `FixedUpdate()`에서 이를 실행

<br>

## 4. 플레이어 좌우 회전

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
	private float _horizontalKeyInput;
	private float _verticalKeyInput;
	private float _xMouseInput;
	private float _yMouseInput;

	[SerializeField]
	private float _walkSpeed;
	private Rigidbody _rigidbody;

	[SerializeField]
	private Camera _camera;
	[SerializeField]
	private float _rotateSpeed;

	private void Awake()
	{
		_rigidbody = GetComponent<Rigidbody>();
	}

	private void Update()
	{
		DetectInput();
		CharacterRotate();
	}

	...

	private void CharacterRotate()
	{
		Vector3 yRotation = new Vector3(0f, _xMouseInput, 0f) * _rotateSpeed;
		_rigidbody.MoveRotation(_rigidbody.rotation * Quaternion.Euler(yRotation));
	}
}
```
매 프레임마다 플레이어의 y축 회전값을 변경

<br>

## 5. 카메라 상하 회전

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
	private float _horizontalKeyInput;
	private float _verticalKeyInput;
	private float _xMouseInput;
	private float _yMouseInput;

	[SerializeField]
	private float _walkSpeed;
	private Rigidbody _rigidbody;

	[SerializeField]
	private Camera _camera;
	[SerializeField]
	private float _rotateSpeed;
	private float _currentCameraRotationX;
	private const float CAMERA_ROTATION_LIMIT = 80;

	...

	private void Update()
	{
		DetectInput();
		CharacterRotate();
		CameraRotate();
	}

	...

	private void CameraRotate()
	{
		float xRotation = _yMouseInput * _rotateSpeed;

		_currentCameraRotationX -= xRotation;
		_currentCameraRotationX = Mathf.Clamp(_currentCameraRotationX, -CAMERA_ROTATION_LIMIT, CAMERA_ROTATION_LIMIT);

		_camera.transform.localEulerAngles = new Vector3(_currentCameraRotationX, 0f, 0f);
	}
}
```
플레이어의 상하 회전은 카메라가 대신한다. <br>
카메라 오브젝트를 받아서 매 프레임마다 카메라의 회전각을 변경해준다.

<br>

## 6. 점프 구현하기

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
	private float _horizontalKeyInput;
	private float _verticalKeyInput;
	private float _xMouseInput;
	private float _yMouseInput;
	private bool _spacebarInput;

	[SerializeField]
	private float _walkSpeed;
	private Rigidbody _rigidbody;

	[SerializeField]
	private Camera _camera;
	[SerializeField]
	private float _rotateSpeed;
	private float _currentCameraRotationX;
	private const float CAMERA_ROTATION_LIMIT = 80;

	[SerializeField]
	private float _jumpPower;
	private bool _isJumping;

	private void Awake()
	{
		_rigidbody = GetComponent<Rigidbody>();
		_isJumping = false;
	}

	...

	private void FixedUpdate()
	{
		Move();
		Jump();
	}

	private void DetectInput()
	{
		_horizontalKeyInput = Input.GetAxisRaw("Horizontal");
		_verticalKeyInput = Input.GetAxisRaw("Vertical");

		_xMouseInput = Input.GetAxisRaw("Mouse X");
		_yMouseInput = Input.GetAxisRaw("Mouse Y");

		_spacebarInput = Input.GetKey(KeyCode.Space);
	}

	...

	private void Jump()
	{
		if (_spacebarInput)
		{
			if (!_isJumping)
			{
				_isJumping = true;
				_rigidbody.AddForce(Vector3.up * _jumpPower, ForceMode.Impulse);
			}
		}
	}

	private void OnCollisionEnter(Collision collision)
	{
		if (collision.gameObject.CompareTag("Ground"))
		{
			_isJumping = false;
		}
	}
}
```
`_spacebarInput`으로 Spacebar 입력을 감지 <br>
`_isJumping`으로 현재 점프하고 있는지를 감지 <br>
만약 점프 뛰고 있지 않다면 `Rigidbody` 컴포넌트에 위 방향으로 힘을 줌 <br>

`OnCollisionEnter()`를 통해 바닥에 닿았다면 `_isJumping`을 `false`로 전환