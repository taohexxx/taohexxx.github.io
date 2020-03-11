---
layout: post
title: "XCode 11编写OpenGL程序"
description: ""
category: opengl
tags: [opengl, xcode, macos]
---
{% include JB/setup %}


## 安装GLEW和GLFW库

```sh
brew install glew
brew install glfw3
```

## 新建XCode工程

选择`macOS` -> `App`。
选择`Objective-C`和`Storyboard`。不能选`XIB`。

## 设置工程

删除`Main.storyboard`、`AppDelegate.h`、`AppDelegate.m`、`ViewController.h`、`ViewController.m`、`main.m`。

## 配置Glad

打开[Glad](http://glad.dav1d.de)在线服务，如图设置：

点击生成并下载`glad.zip`。XCode工程右键菜单`Show in Finder`，工程跟目录下新建`external`子目录，将`glad.zip`解压到其中。包含`include`和`src`两个目录。

### TARGETS编译设置

打开`Build Settings` -> `Header Search Paths`，加入`$(PROJECT_DIR)/external/include`、`$(inherited)`、`/usr/local/include`

打开`Build Phases` -> `Link Binary With Libraries`，从Finder将`/usr/local/Cellar/glfw/3.3.2/lib/libglfw.3.3.dylib`拖入，将`/usr/local/Cellar/glew/2.1.0/lib/libGLEW.2.1.0.dylib`拖入，注意不能拖软链接。

点`+`，搜索`OpenGL`，加入`OpenGL.framework`。

### 源文件

从Finder将项目目录下external/src/glad.c拖进项目（后续选项默认即可）。

新建一个c++文件`main.cpp`。

```cpp
#include <cstdio>
#include <iostream>
#include <glad/glad.h>
#include <GLFW/glfw3.h>


int main(int argc, char **argv) {
  glfwInit();
  glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
  glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
  glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
  glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);
  GLFWwindow *window{glfwCreateWindow(800, 600, "OpenGL", nullptr, nullptr)};
  if (!window) {
    std::cout << "Failed to create GLFW window" << std::endl;
    glfwTerminate();

    return -1;
  }
  glfwMakeContextCurrent(window);

  if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress)) {
    std::cout << "Failed to initialize GLAD" << std::endl;

    return -1;
  }
  glViewport(0, 0, 800, 600);

  void FramebufferSizeCallback(GLFWwindow *window, int width, int height);

  glfwSetFramebufferSizeCallback(window, FramebufferSizeCallback);

  void ProcessInput(GLFWwindow *window);

  while (!glfwWindowShouldClose(window)) {
    ProcessInput(window);
    glClearColor(0.5f, 0.1f, 1.0f, 1.0f);
    glClear(GL_COLOR_BUFFER_BIT);
    glfwSwapBuffers(window);
    glfwPollEvents();
  }

  glfwTerminate();

  return 0;
}

void FramebufferSizeCallback(GLFWwindow *window, int width, int height) {
  glViewport(0, 0, width, height);
}

void ProcessInput(GLFWwindow *window) {
  if (GLFW_PRESS == glfwGetKey(window, GLFW_KEY_ESCAPE))
    glfwSetWindowShouldClose(window, true);
}
```

### 画直线

OpenGL 3画直线

OpenGL 3.2以下

<https://github.com/SonarSystems/OpenGL-Tutorials>

macOS只支持OpenGL 3.2/OpenGL 3.3，不支持画直线，汗

<https://stackoverflow.com/questions/8791531/opengl-3-2-core-profile-gllinewidth>

<https://mattdesl.svbtle.com/drawing-lines-is-hard>

<https://blog.mapbox.com/drawing-antialiased-lines-with-opengl-8766f34192dc>

<https://blog.tammearu.eu/posts/gllines/>

<https://gamedev.stackexchange.com/questions/133208/difference-in-gldrawarrays-and-gldrawelements>

<https://github.com/winduptoy/linebaby>
