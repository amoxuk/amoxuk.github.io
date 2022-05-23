# GIT 拉取子目录

即稀疏检出

GIT从1.7.0版本开始Git提供稀疏检出的功能。所谓稀疏检出就是本地版本库检出时不检出全部，只将指定的文件从本地版本库检出到工作区，而其他未指定的文件则不予检出（即使这些文件存在于工作区，其修改也会被忽略）。

```shell
mkdir [dir-name]
cd [dir-name]
git init
git remote add origin [git-url]
git config core.sparsecheckout true
vi .git/info/sparse-checkout
# 添加子目录的路径
git fetch --depth 1 origin [master]
git checkout [master]
```

---

> [跳转到目录](index.md)
