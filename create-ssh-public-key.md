# 创建SSH Public Key

命令：

```shell
# 生成Key
ssh-keygen -t ed25519 -C "你的邮箱"
# 读取公钥
cat ~/.ssh/id_ed25519.pub

```

使用案例：

```shell
# ssh-keygen -t ed25519 -C "你的邮箱"  ##执行命令
Generating public/private ed25519 key pair.
Enter file in which to save the key (/root/.ssh/id_ed25519): ##回车
Enter passphrase (empty for no passphrase): ##回车
Enter same passphrase again: ##回车
Your identification has been saved in /root/.ssh/id_ed25519
Your public key has been saved in /root/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:xxxxxxxxxxxxxxxxxxxxk 你的邮箱
The key's randomart image is:
+--[ED25519 256]--+
|        ............|
|        ............|
|        ............|
|        ............|
|        ............|
|        ............|
|        ............|
|        ............|
|        ............|
+----[SHA256]-----+

# cat ~/.ssh/id_ed25519.pub ##读取公钥，复制到github中即可。
ssh-ed25519 公钥 你的邮箱

```

---

> [跳转到目录](menu.md)
