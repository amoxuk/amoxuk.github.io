# 镜像资源

>       腾讯软件源 <https://mirrors.cloud.tencent.com/>
>       淘宝软件源 <https://npmmirror.com/mirrors/>
>       阿里云镜像 <https://developer.aliyun.com/mirror/>
>       清华开源镜像站 <https://mirrors.tuna.tsinghua.edu.cn/#>

## 使用腾讯云镜像源加速pip

<details>
    <summary>使用腾讯云镜像源加速pip</summary>
    <pre>
    运行以下命令以使用腾讯云pypi软件源：

    ```shell
    pip install -i https://mirrors.cloud.tencent.com/pypi/simple <some-package>
    ```

    注意：必须加上路径中的simple

    设为默认

    升级 pip 到最新的版本 (>=10.0.0) 后进行配置：

    `pip config set global.index-url https://mirrors.cloud.tencent.com/pypi/simple`
    </pre>
</details>


## 修改Ubuntu的镜像地址为阿里云的镜像

<details>
    <summary>修改Ubuntu的镜像地址</summary>
    <pre>
    ```shell
    sed -i s/http:\/\/cn.archive.ubuntu.com/https:\/\/mirrors.aliyun.com/g /etc/apt/sources.list
    ```
    </pre>
</details>

---

> [跳转到目录](menu.md)
