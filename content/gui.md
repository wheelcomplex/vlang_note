## GUI

按照目前作者的想法是:基于openGL,glad,glfw来创建一个比较轻量的GUI

也有可能采用[https://github.com/floooh/sokol](https://github.com/floooh/sokol ) 来进行GUI以及绘图的开发,等待作者更新ui模块

vui库已经发布,代码库:https://github.com/vlang/ui

### 安装

使用前先安装依赖:

```
macOS:
brew install glfw freetype

Debian/Ubuntu:
sudo apt install libglfw3 libglfw3-dev libfreetype6-dev

Arch/Manjaro:
sudo pacman -S glfw-x11 freetype2

Fedora:
sudo dnf install glfw glfw-devel freetype-devel

Windows:
git clone --depth=1 https://github.com/ubawurinna/freetype-windows-binaries [path to v repo]/thirdparty/freetype/
```

目前ui还是早期版本,更新较快,建议用安装方式1,可以用到最新更新的ui代码

安装方式1:直接从git代码库下载代码,链接到~/.vmodules/ui

```
git clone https://github.com/vlang/ui
ln -s /path/to/ui ~/.vmodules/ui
```

安装方式2:使用vpm安装

```
v install ui
```



### vui涉及到的标准模块

#### gx模块

源代码位置:vlib/gx

主要维护了颜色,文本,字体相关的基础结构体及常量

Color结构体

标准颜色的常量值

Image结构体

TextCfg结构体

#### gl模块

源代码位置:vlib/gl

glad的缩写,GLAD是继GL3W，GLEW之后，当前最新的用来访问OpenGL规范接口的第三方库

OpenGL loading libraries

官方网址为https://glad.dav1d.de/

源代码:https://github.com/Dav1dde/glad

gl模块就是glad库主要的C函数简单封装后,成为V函数

#### glm模块

源代码位置:vlib/glm

OpenGL Mathematics（GLM） - 几何数学库

Math Libraries

OpenGl中在进行图形变换的时候需要使用几何数学库

矩阵变换，四元数，数据打包，随机数，噪声等等

代码:https://github.com/g-truc/glm

#### freetype模块

源代码位置:vlib/freetype

FreeType是一个完全开源的、可扩展、可定制且可移植的字体引擎，它提供TrueType字体驱动的实现统一的接口来访问多种字体格式文件

官网:https://www.freetype.org/

#### stbi模块

源代码位置:vlib/stbi

图像库

源代码:https://github.com/nothings/stb

#### glfm模块

源代码位置:vlib/glfm

GLFW官网链接:https://www.glfw.org/

源代码:https://github.com/glfw/glfw

**GLFW** 是一个专门针对 OpenGL 的 C 语言库，它允许用户创建 OpenGL 上下文并显示窗口，它提供了一些渲染物体所需的最低限度的接口。其用来代替之前的 GLUT 库

Context/Window Toolkits

GLFW使用感受：轻量，简单，好使

glfm模块基本是对C函数进行对应的V函数封装,提供给ui模块使用

#### gg模块

源代码位置:vlib/gg

V 2D/3D graphics library with an OpenGL backend (DirectX, Vulkan, Metal coming soon)

V绘图模块,用来在窗体上绘制各种图形

gg是glad和glfw的缩写

gg其实就是绘图部分的context的作用,觉得可以直接命名为Context更好理解

gg结构体提供进行绘制的相关方法,给全局使用,特别是组件的draw()

gg.draw_triangle() //绘制三角形

gg.draw_triangle_tex() //绘制三角形纹理

gg.draw_rect()	//绘制实心矩形

gg.draw_empty_rect()	绘制空矩形

gg.draw_circle()	//绘制圆形

gg.draw_line()	//绘制直线

...还有好多绘制的功能没有实现



#### 资料参考

OpenGL**（英语：*Open Graphics Library*，译名：**开放图形库**或者“开放式图形库”）是用于[渲染](https://baike.baidu.com/item/渲染)[2D](https://baike.baidu.com/item/2D)、[3D](https://baike.baidu.com/item/3D)[矢量图形](https://baike.baidu.com/item/矢量图形)的跨[语言](https://baike.baidu.com/item/语言)、[跨平台](https://baike.baidu.com/item/跨平台)的[应用程序编程接口](https://baike.baidu.com/item/应用程序编程接口)（API）。这个接口由近350个不同的函数调用组成，用来绘制从简单的图形比特到复杂的三维景象。而另一种程序接口系统是仅用于[Microsoft Windows](https://baike.baidu.com/item/Microsoft Windows)上的[Direct3D](https://baike.baidu.com/item/Direct3D)。OpenGL常用于[CAD](https://baike.baidu.com/item/CAD)、[虚拟现实](https://baike.baidu.com/item/虚拟现实)、科学可视化程序和电子游戏开发。

OpenGL的高效实现（利用了图形加速硬件）存在于[Windows](https://baike.baidu.com/item/Windows)，部分[UNIX](https://baike.baidu.com/item/UNIX)平台和[Mac OS](https://baike.baidu.com/item/Mac OS)。这些实现一般由显示设备厂商提供，而且非常依赖于该厂商提供的硬件

OpenGL是Khronos Group开发维护的一个规范，它主要为我们定义了用来操作图形和图片的一系列函数的API，需要注意的是OpenGL本身并非API。
GPU的硬件开发商则需要提供满足OpenGL规范的实现，这些实现通常被称为“驱动”，它们负责将OpenGL定义的API命令翻译为GPU指令。

OpenGL并非一个能够直接安装的库或包，它只是一个规范。我们只需要安装显卡的驱动即可，因为显卡驱动中就包括了对OpenGL规范的实现

由于 OpenGL 只是一个标准/规范，具体的实现是由驱动开发商针对特定显卡实现的。而 OpenGL 驱动版本众多，它大多数函数的位置都无法在编译时确定下来，需要在运行时查询。所以任务就落在了开发者身上，开发者需要在运行时获取函数地址并将其保存在一个函数指针中供以后使用。但这样写出的代码复杂繁琐，因此我们需要 GLAD,GLAD 是目前最流行的开源库，能帮我们简化这个流程

简写全称

GL:graphics library,open graphics library

GLAD: gl + load

glew: gl + Extension Wrangler

GLFW:gl + frame windows



### 组件

刚发布的版本,目前的组件还比较少

除了最基本的window组件是基于glfw window外,其他组件都是自行绘制的,可以在所有组件代码的draw()函数中看到自行绘制的代码

基本的思路是:使用glfw的window,context,event,然后在窗体上自行绘制所有组件:

以window组件为例,显示通用的组件创建过程

1. 定义window结构体
2. 通过new_window(cfg WindowConfig) window 创建窗体,其中WindowConfig是配置类
3. 在每一个组件中定义draw()函数,包含绘制组件代码
4. 把每一个窗体内的组件都添加到window的children[]中
5. 使用window的ui,也就是UI结构体的实例来进行绘制
6. 最后调用ui.run(window ui.Window)函数,进入for{}无限循环,然后循环调用window中所有children[]中每一个组件的draw()函数,渲染组件,类似实时绘制的效果,最后调用window.ui.gg.render()函数,完成渲染,并监听事件

因为都是采用自行绘制的,所以

才有可能同一套UI代码,除了win,linux,mac外,以后也可以自行绘制成js前端组件,wasm组件,才有可能自行完全控制

才有可能实现响应式UI,监控代码修改,然后实时更新UI,类似swiftUI,响应式UI的实现也挺简单的,因为所有界面上的组件都是实时绘制的

也因为是采用自行绘制的,组件和组件的各种属性,方法,事件都要基于glfm重新定义

#### UI结构体

UI结构体主要包含了:绘制图形的gg,绘制文字的ft,系统剪贴板clipboard

window中的ui用来进行绘制图形,绘制文字,处理剪贴板

一般来说全局只有:1个glfm.window实例,1个ui.window实例,1个ui.UI实例

#### Widget接口

所有的组件都实现了该接口

#### Window窗体



#### Canvas画布



#### Label标签



#### Button按钮



#### Textbox文本框



#### Checkbox复选框



#### Radio单选框



#### Slider滑竿



#### Dropdown下拉菜单



#### Progressbar进度条



#### Picture图像



#### Menu菜单



#### TransitionValue动画





------

### sokol图形库参考

sokol是ui依赖的C图形库,如果想要更清楚理解ui是如何运作的,得掌握一下所依赖的sokol图形库:

官方网址:https://github.com/floooh/sokol

官方DEMO:https://floooh.github.io/sokol-html5/index.html (WASM版本)

官方DEMO源代码:https://github.com/floooh/sokol-samples

作者整体思路文档:https://floooh.github.io/2017/07/29/sokol-gfx-tour.html

官方简介:简单,单文件,跨平台库,可供C/C++使用,C写的

整个库包含这几个C文件,每个C文件可以单独使用:

Cross-platform libraries:

- **sokol_gfx.h**: 3D-API wrapper (GL + Metal + D3D11)
- **sokol_app.h**: app framework wrapper (entry + window + 3D-context + input)
- **sokol_time.h**: time measurement
- **sokol_audio.h**: minimal buffer-streaming audio playback
- **sokol_fetch.h**: asynchronous data streaming from HTTP and local filesystem
- **sokol_args.h**: unified cmdline/URL arg parser for web and native apps

Utility libraries:

- **sokol_imgui.h**: sokol_gfx.h rendering backend for [Dear ImGui](https://github.com/ocornut/imgui)
- **sokol_gl.h**: OpenGL 1.x style immediate-mode rendering API on top of sokol_gfx.h
- **sokol_fontstash.h**: sokol_gl.h rendering backend for [fontstash](https://github.com/memononen/fontstash)
- **sokol_gfx_imgui.h**: debug-inspection UI for sokol_gfx.h (implemented with Dear ImGui)

vui目前使用了这4个,主要是前两个核心文件,已包含在V源代码的thirdparth/sokol中,无需单独下载

**sokol_gfx.h**

- 简单,现代地封装了GLES2/WebGL, GLES3/WebGL2, GL3.3, D3D11 和 Metal
- 提供buffers, images, shaders, pipeline-state-objects 和 render-passes
- 无需控制窗体的创建或者3D API的上下文初始化
- 无需提供着色器方言交叉翻译

**sokol_app.h**

-  统一的应用入口
- 单窗体或画布提供3D渲染
- 3D上下文初始化
- 事件驱动的键盘,鼠标,触摸板输入
- 支持的平台: Win32, MacOS, Linux (X11), iOS, WASM/asm.js, Android (planned: RaspberryPi)
- 支持的3D-APIs: GL3.3 (GLX/WGL), Metal, D3D11, GLES2/WebGL, GLES3/WebGL2

**sokol_gl.h**

OpenGL 1.x样式的立即模式渲染API,基于sokol_gfx.h

**sokol_fontstash.h**

为[fontstash](https://github.com/memononen/fontstash)提供渲染后端

------

