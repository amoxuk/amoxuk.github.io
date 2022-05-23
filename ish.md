# iSH.app - IOS Apple Store可以下载的虚拟开发环境

## 环境说明

> 2022.5.14 iSH v1.2.3 build 298

```shell
amoxuk:~# uname -a 
Linux amoxuk 4.20.69-ish iSH 1.2.3 (298) Dec 17 2021 06:08:24 i686 Linux
```

iSH是一个模拟器，用来在ARM架构的iOS设备上模拟x86架构，让iOS设备在本地运行Linux Shell环境，iSH使用的Linux系统镜像是Alpine Linux，了解Alpine见：<https://alpinelinux.org/>

iSH官方库：<https://github.com/ish-app/ish>

iSH官网：<https://ish.app>

## 修改镜像

修改镜像为阿里云镜像，替换中间的域名即可，`mirrors.aliyun.com`

```shell
amoxuk:~# vi /etc/apk/repositories
amoxuk:~# cat /etc/apk/repositories 
https://mirrors.aliyun.com/alpine/v3.14/main
https://mirrors.aliyun.com/alpine/v3.14/community

amoxuk:~# apk update     # 记得还要更新缓存
```

## 软件安装

python用于ftp安装，共享文件，可以使用ftp插件在vscode中方便同步，也可以通过windows的资源管理器访问ftp，

py3-pip是python的包管理工具。

openssh是ssh的客户端及服务端，远程工具。

openrc：服务管理工具，没有越狱时，无法保持后台，重进需要重启服务，通过该工具可以添加自定义服务和一键启动进程。

注：安装vsftp、samba均不能正常使用。可以安装docker，但是无法启动。ip/ifconfig等命令不能正常使用，无法读取网络配置。

```shell
apk add python3
apk add py3-pip
apk add openssh
apk add openrc
```

## 搭建ftp服务器

这里使用python搭建ftp服务，先使用pip安装pyftpdlib，然后就可以直接起ftp服务了，安装命令：

```shell
# 设置镜像
pip config set global.index-url https://mirrors.cloud.tencent.com/pypi/simple
# 安装ftp库
pip install pyftpdlib
# 启动一个可读可写的，允许匿名访问的ftp服务
python -m pyftpdlib -w

# 接下来就可以使用：ftp://192.168.1.2:2121 对内网的ftp地址进行访问了，默认端口为2121

```

pyftpdlib的参数说明如下，可通过 `-h` 参数进行查看：

```shell
amoxuk:~# python -m pyftpdlib -h
Usage: python -m pyftpdlib [options]

Start a stand alone anonymous FTP server.

Options:
  -h, --help
     show this help message and exit

  -i ADDRESS, --interface=ADDRESS
     specify the interface to run on (default all interfaces)

  -p PORT, --port=PORT
     specify port number to run on (default 2121)

  -w, --write
     grants write access for logged in user (default read-only)

  -d FOLDER, --directory=FOLDER
     specify the directory to share (default current directory)

  -n ADDRESS, --nat-address=ADDRESS
     the NAT address to use for passive connections

  -r FROM-TO, --range=FROM-TO
     the range of TCP ports to use for passive connections (e.g. -r 8000-9000)

  -D, --debug
     enable DEBUG logging evel

  -v, --version
     print pyftpdlib version and exit

  -V, --verbose
     activate a more verbose logging

  -u USERNAME, --username=USERNAME
     specify username to login with (anonymous login will be disabled and password required if supplied)

  -P PASSWORD, --password=PASSWORD
     specify a password to login with (username required to be useful)
```

doc: <https://pyftpdlib.readthedocs.io/en/latest/tutorial.html>

## 将python脚本的ftp添加为服务

编辑添加服务脚本，服务脚本路径如下，内容在后面，然后修改权限，添加到openrc管理：

```shell
amoxuk:~# vi /etc/init.d/pyftp          # 编辑服务脚本
amoxuk:~# chmod +x /etc/init.d/pyftp    # 修改权限
amoxuk:~# rc-update add pyftp           # 添加服务
amoxuk:~# rc                            # 启动所有服务，新的命令为`openrc`，会将没有运行得服务拉起来
```

下文为脚本的内容

```bash
#!/sbin/openrc-run

command="python3"
command_args="-m pyftpdlib -w"
command_background=true
pidfile="/run/pyftp.pid"

start() {
   ebegin "Starting pyftp"
   start-stop-daemon --start --background \
        --exec "$command" \
        --make-pidfile --pidfile $pidfile \
        -- \
        $command_args
   eend $?
}

stop() {
   ebegin "Stopping pyftp"
   start-stop-daemon --stop \
        --exec "$command" \
        --pidfile $pidfile
   eend $?
}

reload() {
    ebegin "Reloading pyftp"
    start-stop-daemon --exec "$command" \
        --pidfile $pidfile \
        -s 1
    eend $?
}
```

doc: <https://wiki.alpinelinux.org/wiki/Writing_Init_Scripts>

## end

然后就可以愉快地使用vscode编辑文档，使用ftp进行文件同步了，可以安装vscode插件更加方便。

---

> [跳转到目录](index.md)
