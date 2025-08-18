---
layout: post
title: "2. Separating Class"
date: 2025-08-18
categories: [uray_engine]
---

# Shader 클래스 분리

`main.cpp`에 셰이더 소스 코드를 로드하는 부분을 분리해서 따로 클래스로 관리한다.

```cpp
#pragma once

#include <string>

#include <glm/glm.hpp>

#include "../config.h"

namespace uray {
class Shader
{
public:
    Shader();
    Shader(const std::string& vertexPath, const std::string& fragmentPath);
    ~Shader();

    void Use() const;

    void SetVec3(const std::string& name, const glm::vec3& val) const;
    void SetVec4(const std::string& name, const glm::vec4& val) const;
    void SetMat4(const std::string& name, const glm::mat4& val) const;

private:
    GLuint _program;
};
} // namespace uray
```

# SpriteRenderer 클래스 분리

스프라이트를 렌더링하는 부분도 따로 클래스로 분리한다.

```cpp
#pragma once

#include <string>

#include "../config.h"

namespace uray {
class SpriteRenderer
{
public:
    SpriteRenderer();
    SpriteRenderer(const std::string& imgPath);
    ~SpriteRenderer();

    void Render();

private:
    unsigned char* _img;
    GLuint _texture;

    GLuint _vao;
    GLuint _vbo;
    GLuint _ebo;
};
} // namespace uray
```

따로 클래스로 빼서 수정된 `main.cpp`는 다음과 같다.

```cpp
int main()
{
    ...

    uray::Shader* spriteShader = new uray::Shader("../src/shader/sprite.vert.glsl", "../src/shader/sprite.frag.glsl");
    uray::SpriteRenderer* spriteRenderer = new uray::SpriteRenderer("../resource/container.jpg");

    // Render Loop
    while (!glfwWindowShouldClose(window)) {
        ProcessInput(window);

        glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);

        spriteShader->Use();
        spriteRenderer->Render();

        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    delete spriteShader;
    delete spriteRenderer;

    // Clean GLFW's resources.
    glfwTerminate();
    return 0;
}
```

---
<br>

# To-do Features

- 카메라 뷰 적용