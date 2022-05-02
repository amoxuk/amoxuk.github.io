# 镜像资源

* 腾讯软件源 [https://mirrors.cloud.tencent.com](https://mirrors.cloud.tencent.com){target="_blank"}
* 淘宝软件源 <https://npmmirror.com/mirrors/>
* 阿里云镜像 <https://developer.aliyun.com/mirror/>
* 清华开源镜像站 <https://mirrors.tuna.tsinghua.edu.cn/#>

## python

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

## maven

## go

---

> [跳转到目录](menu.md)
