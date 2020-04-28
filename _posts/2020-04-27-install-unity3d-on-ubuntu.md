---
layout: post
title: 在 Ubuntu 18.04 上安装 Unity3D
categories: [game]
tags: [unity3d, linux, ubuntu]
---

本文包括在 Ubuntu 上安装 Unity3D 的过程，遇到的问题及解决办法。先安装 UnityHub，再安装 Unity，就可以使用 Unity 了。把官方教学项目 FPS Microgame 构建为 WebGL 平台程序后，可以直接在浏览器中试玩游戏。

# 安装 UnityHub

下载最新版本 UnityHub：

https://public-cdn.cloud.unity3d.com/hub/prod/UnityHub.AppImage

国内地址：[https://public-cdn.cloud.unitychina.cn/hub/prod/UnityHub.AppImage](https://public-cdn.cloud.unitychina.cn/hub/prod/UnityHub.AppImage)

下载完毕后，赋予执行权限

```sh
chmod +x UnityHub.AppImage
```

然后就可以直接执行此程序

参考 ：[Install Unity3D on Linux](https://www.linuxdeveloper.space/install-unity-linux/)

# 安装 Unity

当前在 Ubuntu 下，无法脱离 UnityHub 单独运行 Unity，需要 UnityHub 激活和管理许可证（License）后才能正常使用 Unity。

有几种安装方式可以选择

## 方式1：使用 UnityHub 安装

直接使用 UnityHub 安装相应版本的 Unity，这种方式安装只有有限的几个版本可以选择，如果想要安装其他版本，需要进行手动安装，然后添加到 UnityHub 中。

由于需要下载的文件巨大（几个GB），国内网络环境下可能会频繁遇到下载中断、下载速度慢的问题，可以在执行 UnityHub 时挂上代理，加快下载速度。

## 方式2：手动安装

也可以使用 UnitySetup 手动安装 Unity。按着官方论坛 [Unity on Linux: Release Notes and Known Issues](https://forum.unity.com/threads/unity-on-linux-release-notes-and-known-issues.350256) 的说明，下载最新版本的 UnitySetup-x.x.x 后，运行 UnitySetup 进行安装。下载 UnitySetup 时可能也需要挂上代理。

### 下载其他版本的 UnitySetup

如果论坛中没有你需要的版本，可以通过替换下载链接中的参数来下载其他版本。论坛中官方人员给出的下载地址，有一定的规律。以 **2019.1.0f2** 版本为例，下载地址为：

http://beta.unity3d.com/download/**292b93d75a2c**/UnitySetup-**2019.1.0f2** 

国内地址：[https://download.unitychina.cn/download_unity/292b93d75a2c/UnitySetup-2019.1.0f2](https://download.unitychina.cn/download_unity/292b93d75a2c/UnitySetup-2019.1.0f2)

只需要替换链接中的 Hash 和版本号就可以下载相应的版本。

假如我们想下载 **Unity 2019.3.11**，根据官方[下载存档列表](https://unity3d.com/get-unity/download/archive)找到相应版本，Chrome浏览器在 **Unity Hub** 链接（按钮）上鼠标右键**审查**（Firefox 鼠标右键‘检查元素’），得到版本 UnityHub 安装链接为 **unityhub://2019.3.11f1/ceef2d848e70**，链接中 **2019.3.11f1** 和 **ceef2d848e70** 就是我们需要的参数，替换参数后的链接为：

http://beta.unity3d.com/download/**ceef2d848e70**/UnitySetup-**2019.3.11f1**

国内地址：[https://download.unitychina.cn/download_unity/ceef2d848e70/UnitySetup-2019.3.11f1](https://download.unitychina.cn/download_unity/ceef2d848e70/UnitySetup-2019.3.11f1)

就可以下载到**2019.3.11f1**版本的 UnitySetup。

官方下载存档列表备份：
- 备份1：[旧版本](https://gitlab.com/gableroux/unity3d/-/blob/master/ci-generator/unity_versions.old.yml) ， [当前版本](https://gitlab.com/gableroux/unity3d/-/blob/master/ci-generator/unity_versions.yml)
- [备份2](https://github.com/neogeek/get-unity/blob/master/data/editor-installers.json)

### 解决 UnitySetup 下载问题

运行 UnitySetup 下载相关安装包时，可能会遇到中断和下载太慢的问题。UnitySetup 会顺序下载已经选定的组件，如果想对下载进行加速，可以在外部先下载好相关安装包，再运行 UnitySetup，此时会跳过下载步骤，直接开始安装 Unity。

根据 UnitySetup 启动时请求的配置文件，我们可以**按上一步**的方式，替换参数获得对应版本的组件配置文件，自己下载。以 **2019.1.0f2** 版本为例，UnitySetup 启动时会请求以下链接获取配置文件，进行后续下载和安装过程：

https://netstorage.unity3d.com/unity/**292b93d75a2c**/unity-**2019.1.0f2**-linux.ini

国内地址：[https://netstorage.unitychina.cn/unity/292b93d75a2c/unity-2019.1.0f2-linux.ini](https://netstorage.unitychina.cn/unity/292b93d75a2c/unity-2019.1.0f2-linux.ini)

我们把其中的参数替换为 **2019.3.11f1** 的后，就可以得到对应版本的组件配置信息：

https://netstorage.unity3d.com/unity/**ceef2d848e70**/unity-**2019.3.11f1**-linux.ini

国内地址：[https://netstorage.unitychina.cn/unity/ceef2d848e70/unity-2019.3.11f1-linux.ini](https://netstorage.unitychina.cn/unity/ceef2d848e70/unity-2019.3.11f1-linux.ini)

然后进行多线程下载：

```sh
# 下载配置文件
curl -L https://netstorage.unity3d.com/unity/ceef2d848e70/unity-2019.3.11f1-linux.ini -o unity-2019.3.11f1-linux.ini
# 通过代理下载配置文件
#proxychains curl -L https://netstorage.unity3d.com/unity/ceef2d848e70/unity-2019.3.11f1-linux.ini -o unity-2019.3.11f1-linux.ini
# 把 unity-2019.3.11f1-linux.ini 需要下载的组件写入下载列表文件中, 同样需要参数替换
echo '
https://netstorage.unity3d.com/unity/ceef2d848e70/LinuxEditorInstaller/Unity.tar.xz
https://netstorage.unity3d.com/unity/ceef2d848e70/MacEditorTargetInstaller/UnitySetup-Android-Support-for-Editor-2019.3.11f1.pkg
https://netstorage.unity3d.com/unity/ceef2d848e70/LinuxEditorTargetInstaller/UnitySetup-Linux-IL2CPP-Support-for-Editor-2019.3.11f1.tar.xz
https://netstorage.unity3d.com/unity/ceef2d848e70/MacEditorTargetInstaller/UnitySetup-Windows-Mono-Support-for-Editor-2019.3.11f1.pkg
https://netstorage.unity3d.com/unity/ceef2d848e70/LinuxEditorTargetInstaller/UnitySetup-WebGL-Support-for-Editor-2019.3.11f1.tar.xz
' > list.txt
# 使用 aria2c 进行多线程下载
aria2c --auto-file-renaming false -m 0 -i list.txt
# 下载完毕后，运行 UnitySetup，下载目录指向当前已经下载好的组件目录，就可以直接跳过下载进行安装了   
```

国内链接版本

```sh
# 下载配置文件
curl -L https://netstorage.unitychina.cn/unity/ceef2d848e70/unity-2019.3.11f1-linux.ini -o unity-2019.3.11f1-linux.ini
# 把 unity-2019.3.11f1-linux.ini 需要下载的组件写入下载列表文件中, 同样需要参数替换
echo '
https://netstorage.unitychina.cn/unity/ceef2d848e70/LinuxEditorInstaller/Unity.tar.xz
https://netstorage.unitychina.cn/unity/ceef2d848e70/MacEditorTargetInstaller/UnitySetup-Android-Support-for-Editor-2019.3.11f1.pkg
https://netstorage.unitychina.cn/unity/ceef2d848e70/LinuxEditorTargetInstaller/UnitySetup-Linux-IL2CPP-Support-for-Editor-2019.3.11f1.tar.xz
https://netstorage.unitychina.cn/unity/ceef2d848e70/MacEditorTargetInstaller/UnitySetup-Windows-Mono-Support-for-Editor-2019.3.11f1.pkg
https://netstorage.unitychina.cn/unity/ceef2d848e70/LinuxEditorTargetInstaller/UnitySetup-WebGL-Support-for-Editor-2019.3.11f1.tar.xz
' > list.txt
# 使用 aria2c 进行多线程下载
aria2c --auto-file-renaming false -m 0 -i list.txt
# 下载完毕后，运行 UnitySetup，下载目录指向当前已经下载好的组件目录，就可以直接跳过下载进行安装了   
```

# 测试安装效果

Unity 安装完毕后，运行 UnityHub，在 Learn - Project 下选择一个教学项目进行测试，这里下载了 FPS Microgame（此项目需要安装 WebGL Build Support 组件）。下载完毕后，在 UnityHub 中创建一个新的 3D 项目，然后将 FPS Microgame 导入到新项目中（可能在 /tmp/fps-microgame.unitypackage 或者 ~/.local/share/unity3d/Asset Store-5.x/Unity Technologies/Project/fps-microgame.unitypackage）。

先点击菜单栏中的 Play 按钮，测试游戏能否正常运行。可以运行后，再点击菜单栏中 File -> Build Settings 设置目标平台为 WebGL，然后构建并运行游戏，就可以在**浏览器**中玩游戏了。也可以切换构建平台到 PC, Mac & Linux，然后选择 Linux，构建本地应用。

## 新手教程

如果是刚开始学习 Unity，可以在菜单栏中 Tutorials -> Show Tutorials 打开内置教程，一步一步学习如何使用 Unity。

# 遇到的问题

## 网络，网络，网络

从 unity.com 上下载时，大部分资源的 CDN 都在国外，国内下载过程中随时可能会遇到中断或者速度慢的情况，这时最好中断下载，挂上代理再下载。也可以使用国内链接来下载。

## 帐号注册和登录

运行 Unity 时可能会遇到需要登录 Unity Id 的情况，这时可以挂代理在 unity.com 上申请帐号，不挂代理的话会根据 ip 分配到 unity.cn 上申请。

# 参考资料

- [Unity on Linux: Release Notes and Known Issues](https://forum.unity.com/threads/unity-on-linux-release-notes-and-known-issues.350256)
- [How to install Unity3D on Ubuntu 18.04?](https://askubuntu.com/questions/1077816/how-to-install-unity3d-on-ubuntu-18-04)
- [Install Unity3D on Linux](https://www.linuxdeveloper.space/install-unity-linux/)
- [Unity download archive](https://unity3d.com/get-unity/download/archive)
- [unity3d editor-installers.json - github](https://github.com/neogeek/get-unity/blob/master/data/editor-installers.json)
- [unity_versions.yml - gitlab](https://gitlab.com/gableroux/unity3d/-/blob/master/ci-generator/unity_versions.yml)
- [unity_versions.old.yml - gitlab](https://gitlab.com/gableroux/unity3d/-/blob/master/ci-generator/unity_versions.old.yml)
- [https://unity.cn/releases](https://unity.cn/releases)
