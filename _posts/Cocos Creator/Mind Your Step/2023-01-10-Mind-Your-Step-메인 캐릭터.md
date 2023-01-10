---
title: "Mind Your Step 메인 캐릭터"
excerpt: "Mind Your Step 메인 캐릭터"

categories:
  - Mind Your Step
tags:
  - Cocos Creator
  - Mind Your Step

toc: true
toc_sticky: true

date: 2023-01-10
last_modified_at: 2023-01-10

use_math: true
---

## New Project

![image](https://user-images.githubusercontent.com/85764911/211514677-ac06ac10-4195-4f4e-9a1d-77b41d58a18a.png)

**MindYourStep** 이름을 가진 새 프로젝트를 생성한다.

<br>

## Creating game scene

**Cocos Creator**에서 **Scene**은 개발 도중 필요한 게임 콘텐츠들을 모아놓은 곳이며, 플레이어에게 게임 콘텐츠들을 보여주는 컨테이너이다. <br>
게임 씬은 일반적으로 다음과 같은 컴포넌트들을 포함한다.

- Scene objects
- Roles
- User interface elements
- Game logic, in the form of scripts, attached to Scene Nodes as Components

따라서, 플레이어가 게임을 실행하면, 게임 씬이 로드될 것이다. <br>
게임 씬이 로드된 후에, 컴포넌트로 포함된 스크립트들이 자동적으로 실행될 것이다.

![image](https://user-images.githubusercontent.com/85764911/211516319-7488736b-4e27-4853-9391-859be0172f7e.png)

다음과 같이 **Assets**에서 **Scenes** 이름으로 새 폴더를 만든다.

![image](https://user-images.githubusercontent.com/85764911/211516584-4e4cef1d-a5b1-4734-b384-5e304f9d9a67.png)

폴더 안에 **Main** 이름으로 새로운 씬을 만든다. <br>
**Main** 씬을 더블클릭해서 **Hierarchy** 패널에 **Main** 씬을 연다.

![image](https://user-images.githubusercontent.com/85764911/211517152-5578d9e1-60ab-4a1b-813c-f46cfc9a1359.png)

다음과 같이 하이어라키 패널에 **Main** 씬이 로드된 것을 볼 수 있다.

<br>

## Adding a Road

![image](https://user-images.githubusercontent.com/85764911/211517893-350fe770-f434-495b-bfd2-14f584970561.png)

Main 씬에서 Cube를 생성한다.

![image](https://user-images.githubusercontent.com/85764911/211518051-44188fd6-8722-4305-8242-064129e96afd.png)

Ctrl + D를 통해 두 개의 Cube 복제본을 더 생성한다. <br>
각 Cube의 Position을 변경한다.

> 첫 번째 큐브: (0, -1.5, 0) <br>
> 두 번째 큐브: (1, -1.5, 0) <br>
> 세 번째 큐브: (2, -1.5, 0)

![image](https://user-images.githubusercontent.com/85764911/211518553-58c0e5f5-7ace-49bf-83a0-8f1e4caca9e3.png)

결과는 위와 같다.

<br>

## Main Character

![image](https://user-images.githubusercontent.com/85764911/211520326-28554b09-5031-41cf-9574-cbd0c0f7aee5.png)

하이어라키 탭에서 **Player** 이름으로 빈 노드를 생성한다.

![image](https://user-images.githubusercontent.com/85764911/211520956-8e300248-46c0-4ef0-bd44-9d07a6daf01d.png)

**Player** 빈 노드의 자식으로 **Body** 이름으로 캡슐 오브젝트를 생성한다.

<br>

## Scripting Main Character

![image](https://user-images.githubusercontent.com/85764911/211637298-ab9147ea-02d3-43f6-97a9-9ea69c139f2b.png)

플레이어를 움직일 수 있게 하기 위해 **PlayerController** 이름으로 타입스크립트를 생성한다.

### 📝PlayerController.ts
```ts
import { _decorator, Component, Vec3, input, Input, EventMouse, Animation } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('PlayerController')
export class PlayerController extends Component {
  private _startJump: boolean = false;
  private _jumpStep: number = 0;
  private _curJumpTime: number = 0;
  private _jumpTime: number = 0.1;
  private _curJumpSpeed: number = 0;
  private _curPos: Vec3 = new Vec3();
  private _deltaPos: Vec3 = new Vec3(0, 0, 0);
  private _targetPos: Vec3 = new Vec3();
  private _isMoving = false;

  start() {
    input.on(Input.EventType.MOUSE_UP, this.onMouseup, this);
  }

  onMouseup(event: EventMouse) {
    if (event.getButton() === 0) {
      this.jumpByStep(1);
    } else if (event.getButton() === 2) {
      this.jumpByStep(2);
    }
  }

  jumpByStep(step: number) {
    if (this._isMoving) {
      return;
    }
    
    this._startJump = true;
    this._jumpStep = step;
    this._curJumpTime = 0;
    this._curJumpSpeed = this._jumpStep / this._jumpTime;
    this.node.getPosition(this._curPos);
    Vec3.add(this._targetPos, this._curPos, new Vec3(this._jumpStep, 0, 0));

    this._isMoving = true;
  }

  onOnceJumpEnd() {
    this._isMoving = false;
  }

  update(deltaTime: number) {
    if (this._startJump) {
      this._curJumpTime += deltaTime;
      if (this._curJumpTime > this._jumpTime) {
        // end
        this.node.setPosition(this._targetPos);
        this._startJump = false;
        this.onOnceJumpEnd();
      } else {
        // tween
        this.node.getPosition(this._curPos);
        this._deltaPos.x = this._curJumpSpeed * deltaTime;
        Vec3.add(this._curPos, this._curPos, this._deltaPos);
        this.node.setPosition(this._curPos);
      }
    }
  }
}
```

`cc class`는 직렬화 등을 위해 필요하므로, `cc class`가 선언되지 않은 컴포넌트는 불완전하다. <br>
물론, `cc class`가 없으면 노드로 추가할 수도 없다.

`start()` 메서드에서는 특정한 입력 이벤트를 등록하기 위해 `input.on()`를 사용한다.

`onMouseup()` 메서드는 마우스가 눌렸을 때의 이벤트 메서드이다.

`jumpByStep()` 메서드에서는 우선 캐릭터가 움직이고 있을 경우, 함수를 종료한다. <br>
움직이고 있지 않을 경우에는, 캐릭터가 점프를 할 때의 여러가지 조건을 처리한다. <br>
`Vec3.add()` 메서드에서는 `_targetPos`를 현재 위치에서 `_jumpStep`만큼 간 위치를 `Vec3` 형으로 대입한다.

`onOnceJumpEnd()` 메서드에서는 점프가 종료됐을 때의 조건을 처리한다.

`update()` 메서드는 Unity와 비슷하게 프레임마다 메서드의 내용을 처리한다. <br>
만약 플레이어가 점프하고 있을 경우, 점프에 걸리는 시간을 체크하여 `_jumpTime`과 비교하여 점프를 종료할 때와, 진행 중일 때를 나눈다. <br>

점프가 종료됐을 때는 플레이어의 위치를 `_targetPos`로 바꾸고 점프 처리를 종료한다. <br>
점프가 진행 중일 때는 플레이어의 현재 위치를 계속해서 `_deltaPos`만큼 더하여 갱신시킨다.

<br>

![image](https://user-images.githubusercontent.com/85764911/211643729-31391e36-bec3-4b1b-a468-00f8319f2c34.png)

완성된 **PlayerController.ts** 스크립트를 플레이어 노드의 인스펙터에 컴포넌트로 추가한다.

<br>

## 카메라 설정

런타임 중에 오브젝트가 잘 움직이는지 보기 위해, **Camera** 오브젝트의 **Position**을 $(0, 0, 13)$으로, **ClearColor**를 $(50, 90, 255, 255)$로 설정한다.

![image](https://user-images.githubusercontent.com/85764911/211644527-de94cf77-19d1-4462-a0cd-39a3451a6928.png)

<br>

## 결과

![image](https://user-images.githubusercontent.com/85764911/211644637-e7766df9-496b-4ee2-821c-ba02bbf09ce2.png)