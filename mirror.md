# 镜像资源

* 腾讯软件源 <a href="https://mirrors.cloud.tencent.com" target="_blank">https://mirrors.cloud.tencent.com</a>
* 淘宝软件源 <a href="https://npmmirror.com/mirrors/" target="_blank">https://npmmirror.com/mirrors/</a>
* 阿里云镜像 <a href="https://developer.aliyun.com/mirror/" target="_blank">https://developer.aliyun.com/mirror/</a>
* 清华开源镜像站 <a href="https://mirrors.tuna.tsinghua.edu.cn/" target="_blank">https://mirrors.tuna.tsinghua.edu.cn/</a>
* microsoft <a href="https://packages.microsoft.com/" target="_blank">https://packages.microsoft.com/</a>

## python


windows whl库：<https://www.lfd.uci.edu/~gohlke/pythonlibs>

使用腾讯云镜像源加速pip

临时使用：运行以下命令以使用腾讯云pypi软件源：

```shell
pip install -i https://mirrors.cloud.tencent.com/pypi/simple <some-package>
```

设为默认
命令配置：支持pip(>=10.0.0)

```shell
pip config set global.index-url https://mirrors.cloud.tencent.com/pypi/simple
```


## ubuntu

修改Ubuntu的软件源地址为阿里云的镜像

修改`/etc/apt/sources.list`，替换地址

```shell
sed -i s/http:\/\/cn.archive.ubuntu.com/https:\/\/mirrors.aliyun.com/g /etc/apt/sources.list
```

## docker

## npm

```shell
npm config set registry http://mirrors.cloud.tencent.com/npm/
```

## node

nvm:  

```shell 
nvm node_mirror https://npm.taobao.org/mirrors/node
nvm npm_mirror https://npm.taobao.org/mirrors/npm

nvm install xx.xx.x
nvm uninstall xx.xx.x

nvm use xx.xx.x
```

## gradle
```
# RN android\gradle\wrapper\gradle-wrapper.properties

https://mirrors.cloud.tencent.com/gradle/

```

## maven

## go

## android

https://developer.android.google.cn/studio?hl=zh-cn

从 Android Studio 下载页面中下载最新的“command line tools only”软件包，然后将其解压缩。
将解压缩的 cmdline-tools 目录移至您选择的新目录，例如 android_sdk。这个新目录就是您的 Android SDK 目录。
在解压缩的 cmdline-tools 目录中，创建一个名为 latest 的子目录。
将原始 cmdline-tools 目录内容（包括 lib 目录、bin 目录、NOTICE.txt 文件和 source.properties 文件）移动到新创建的 latest 目录中。现在，您就可以从这个位置使用命令行工具了。

```shell

sdkmanager "platform-tools" "platforms;android-28"
sdkmanager "platforms" "platforms;android-31"

```


---

> [跳转到目录](index.md)
