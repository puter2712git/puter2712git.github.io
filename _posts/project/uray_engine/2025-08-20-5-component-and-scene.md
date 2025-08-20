---
layout: post
title: "5. Component and Scene"
date: 2025-08-20
categories: [uray_engine]
---

# `Component` system

`Component`는 `Unit`에 연결돼서 게임 루프마다 각자의 프로세스를 실행한다. `Unit`의 `Update()`가 실행되면 연결되어 있는 모든 `Component`의 프로세스가 동작하도록 구성한다.

```cpp
// unit.h
#pragma once

#include <string>
#include <vector>

#include <glm/glm.hpp>

namespace uray {
class Component;

class Unit
{
public:
    ...
    void Update();

    void AddComponent(Component* component);

    ...

private:
    std::vector<Component*> _components;

    ...
};
} // namespace uray
```

```cpp
// component.h
#pragma once

namespace uray {
class Component
{
public:
    Component();
    ~Component();

    void SetActive(bool active)
    {
        if (_isActive == active)
            return;

        _isActive = active;
        if (_isActive)
            OnEnable();
        else
            OnDisable();
    }
    bool IsActive() const { return _isActive; }

    virtual void Update() = 0;
    virtual void OnEnable() = 0;
    virtual void OnDisable() = 0;

protected:
    bool _isActive;
};
} // namespace uray
```

<br>

# `Scene`

`Scene`은 여러 개의 `Unit`들이 모인 집합이다. `Scene`이 `Update()`되면 씬이 포함하고 있는 모든 `Unit`이 `Update()`를 호출한다.

```cpp
#pragma once

#include <string>
#include <vector>

namespace uray {
class Unit;

class Scene
{
public:
    Scene(const std::string& name = "NewScene");
    ~Scene();

    void Update();

    void AddUnit(Unit* unit);

private:
    std::string _name;

    std::vector<Unit*> _units;
};
} // namespace uray
```

---
<br>

# To-do Features

- **Asset** system 구현
- **Scene** 데이터 직렬화