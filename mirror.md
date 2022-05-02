# 镜像资源

* 腾讯软件源 <a href="https://mirrors.cloud.tencent.com" target="_blank">https://mirrors.cloud.tencent.com</a>
* 淘宝软件源 <a href="https://npmmirror.com/mirrors/" target="_blank">https://npmmirror.com/mirrors/</a>
* 阿里云镜像 <a href="https://developer.aliyun.com/mirror/" target="_blank">https://developer.aliyun.com/mirror/</a>
* 清华开源镜像站 <a href="https://mirrors.tuna.tsinghua.edu.cn/" target="_blank">https://mirrors.tuna.tsinghua.edu.cn/</a>

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
