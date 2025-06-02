## 在 Vscode 上用 Twee3 插件

#### 前言

本教程与其它教程不同，主要是讲如何在Vs code上正确使用 [Twee 3 Language Tools ](https://marketplace.visualstudio.com/items?itemName=cyrusfirheir.twee3-language-tools) 和 [Twine (Twee 3)](https://marketplace.visualstudio.com/items?itemName=StephenGranade.twine-twee-language) 两个插件，因此可能会有疏漏，还请见谅。

---

#### 目录

[TOC]

---

#### 准备工作

> 注意：请确保已安装 [Visual Studio Code](https://code.visualstudio.com/)
>
> 本教程不考虑Tweego，因为Twine自动下载已经满足需求。

###### Vscode 插件

- Chinese (Simplified) (简体中文) Language Pack for Visual Studio Code。
- Twee 3 Language Tools：提供高级功能（故事地图、IFID生成等）。
- Twine (Twee 3)：基础语法高亮和编译。
- Live Server（选装）：用于实时调试。

<img src=".\images\3.png" alt="3.png" style="zoom:50%;" />

###### 故事格式 StoryFormat

> 在 **tweego/Twine** 生态中，故事格式是决定互动小说如何**运行和呈现**的核心引擎，它包含：
>
> - **渲染逻辑**（如何解析故事文本和指令）
> - **用户界面**（导航按钮、进度条等）
> - **功能支持**（变量、条件分支、存档等）
> - **样式设计**（CSS 视觉风格）

本次我们以 [SugarCube](https://www.motoslave.net/sugarcube/) 举例，你也可以选择其它类型的故事格式。

---

#### 起步

###### 新建文件夹并作为工作文件夹

首先，你需要在任一个目录新建文件夹，并记住它的目录地址。

然后返回到Vs code，在 ***欢迎*** 页面点击**打开文件夹**；

抑或在菜单栏处，点击 ***文件*** 后再点击**打开文件夹**。

<img src=".\images\1.png" alt="1.png" style="zoom:50%;" />

找到你新建的文件夹，并**点击“选择文件夹”**，将其设置为你的工作文件夹。

<img src=".\images\2.png" alt="2.png" style="zoom:50%;" />

###### 生成Twee目录

首先，一个正常Twee目录格式是

```资源管理器
EXAMPLE/
├── .storyformats/
├── build/
├── include/
└── src/
```

你可以选择手动新建这些文件夹，不过为了节约时间，我们可以在Vs Code左侧的资源管理器中（显示资源管理器 Ctrl+B）直接新建一个Twee文件，右下角会弹出提示，询问你是否直接生成这些文件夹，选择 **Yes** 即可

<img src=".\images\4.png" alt="4.png" style="zoom:50%;" />

然后再把刚才新建的twee文件拖动到src即可。



###### 文件目录介绍

**.storyformats**：既**故事格式**，Twee在编译时会根据**StoryData**中的**format**和**format-version**去寻找.storyformat下的故事格式，若没有对应的故事格式，则会报错。

**build**：**构造**抑或**输出**文件夹，生成的html文件均在此处。

**include**：这个文件夹一般存放img，js文件。

**src**：存放twee文件的主要文件夹，若是twee文件在文件夹外部则无法识别。

---

#### Twee文件

在设置好一切后，我们在src目录中打开一个twee文件。

一般情况下，它都会是一片空白，因此我们需要将以下代码复制粘贴：

```Twee
:: StoryData
{
    "ifid": "",
    "format": "",
    "format-version": "",
    "start": "helloworld"
}

:: StoryTitle

New Twee

:: helloworld

Hello World
```



###### StoryData 解释

- ifid：标识符，类似于B站的视频id
- format：故事格式名字，一定要是**全小写！**
- format-version：故事格式版本号，可以在对应故事格式的官网上寻找
- start：故事开始的地方，**必须与段落的名字一模一样**

你可以直接使用基于**sugarcube 2.37.3**故事格式代码

```Twee
{
    "ifid": "",
    "format": "sugarcube",
    "format-version": "2.37.3",
    "start": "helloworld"
}
```



###### 关于 Storyformat

如果你未曾在.storyformat目录中放置任一一个故事格式文件，或者是与storydata中标识的版本或版本号不同，它都会提示你是否下载对应故事格式的副本，你可以直接选择**Download**。

<img src=".\images\5.png" alt="5.png" style="zoom:50%;" />

或者你可以复制你的故事格式到.storyformat中，然后重新填写storydata的内容。

注意，.storyformat目录下仅支持**英文小写、阿拉伯数字和英语划线**，像“sugarcube-2.37.3”直接放进目录下将不被识别，只有”sugarcube-2-37-3“才会被识别。

除此之外，所有的故事格式**尽可能使用具体版本号**，像”sugarcube-2“即便被识别也会提示*“该代码仅支持故事格式高版本，而你的版本号是2”*。

<img src=".\images\7.png" alt="7.png" style="zoom:50%;" />



###### 关于ifid

你可以直接使用Vs code命令行生成一个ifid，具体操作如下。

1. 点击左下角齿轮按钮，打开命令面板 或 Ctrl + Shift + P 打开命令面板
2. 输入twee3直至看到`Twee3 Language Tools:Generate IFID`，选中，点击/回车确认
    <img src=".\images\6.png" alt="6.png" style="zoom:50%;" />
3. 将生成的IFID复制到Storydata的IFID中。

一般情况，它可以在后续输出html文件时自动生成ifid，但为了图省事，建议直接手动生成一个ifid。

---

#### 最后输出

当你的故事完成时或者需要调试时，你可在命令面板中输入

```Vs code
twine(twee 3) language:build game(test mode) //构建调试游戏
twine(twee 3) language:build game //构建游戏
```

然后你可以在build文件夹下找到输出的html文件。

你可以直接在游览器打开它。



###### 关于调试

输出为调试(test mode)时，你可以在你的游戏页面看到调试块。

<img src=".\images\8.png" alt="8.png" style="zoom:50%;" />



###### 使用Live Server

为了能够方便调试，实现实时更新，我们需要借助该工具。

在命令面板中输入`live server:open with live server`便可以开启实时服务器。

当你每一次构建时，html页面都将自动刷新。

<img src=".\images\9.png" alt="9.png" style="zoom:50%;" />

> 开启前，请选择html文件，然后Ctrl + Shift + P 打开命令面板，输入`live server:open with live server`开启服务器，若成功启动，右下角将弹出“Server is Started at port：xxxx”，然后游览器将会自动打开。
>
> 如果你显示的不是游戏页面而是文件夹选择目录，请重新按照上述所说的重新操作一遍，或者根据目录找到你的html文件。

---

#### 好处

|       场景       |            原生Tweego            |                   VS Code                    |
| :--------------: | :------------------------------: | :------------------------------------------: |
| **编写复杂分支** |      需手动记忆语法，易出错      |        自动补全宏命令，分支逻辑可视化        |
| **调试变量错误** |    编译后刷新浏览器查看控制台    |         实时变量高亮 + 内联错误提示          |
|  **多文件协作**  |        需切换目录查找文件        | 全局搜索跨文件跳转（如 `$character` 定义点） |
|   **样式调整**   | 修改 CSS → 编译 → 刷新浏览器循环 |           直接编辑 CSS 并实时预览            |
|   **版本回退**   |           依赖手动备份           |        Git 时间线视图精确还原历史版本        |

#### 命令面板-命令解释

**仅包括常用的**

| 命令                                             | 翻译                 |
| ------------------------------------------------ | -------------------- |
| `twee3LanguageTools: generate ifid`              | 生成IFID             |
| `twee3LanguageTools: set passage as story start` | 设为故事起点         |
| `twee3LanguageTools: focus passage in story map` | 在故事地图中聚焦段落 |
| `twee3LanguageTools: show story map`             | 显示故事地图         |

| 命令                                       | 翻译                 |
| ------------------------------------------ | -------------------- |
| `>twine(twee3) language: build`            | 构建游戏             |
| `>twine(twee3) language: build(test mode)` | 构建游戏（测试模式） |
| `>twine(twee3) language: run`              | 运行游戏             |


---

#### 版权声明

本文《在 Vscode 上用 Twee3 插件》由 [GrassJiang](https://space.bilibili.com/229408320?) 创作，由 [JiangzhiX](https://github.com/Jiangzhix) 负责上传，采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。  
您可自由**共享**和**改编**，但需：  
① 标注作者姓名 ② 非商业使用 ③ 相同方式共享  
[![CC BY-NC-SA 4.0](https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/) 