# Hexo 部署到 Github

## 安装hexo-deployer-git插件

```shell
npm install hexo-deployer-git --save
```

## 配置文件

./_config.yml下
修改,注意：地址为你clone的地址，ssh对应需要ssh-key，https需要在地址上配置账号和http-password.

```yaml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
    type: git
    repo: git@github.com:amoxuk/amoxuk.github.io.git
    branch: master
```

## 发布

```shell
hexo d -g
```

## 修改ICO（页面图标）

放置一张合适的图标图片到.\themes\你当前使用的主题\source下面，修改配置文件，路径：.\themes\你当前使用的主题\_config.yml

```yaml
favicon: /favicon.ico
```

如果文件名字为：favicon.ico，则不需要任何配置，自动部署就生效。

---

> [跳转到目录](menu.md)
