# 覆盖GIT的历史提交记录

1、使用当前的文件创建一个新的分支`latest_branch`。

```shell
git checkout --orphan latest_branch
```

2、添加所有文件

```shell
git add -A
```

3、提交更改

```shell
git commit -am "msg"
```

4、删除分支

```shell
git branch -D gh-pages
```

5、将当前分支重命名

```shell
git branch -m gh-pages
```

6、最后提交到github，强制更新存储库

```shell
git push -f origin gh-pages
```

---

> [跳转到目录](menu.md)
