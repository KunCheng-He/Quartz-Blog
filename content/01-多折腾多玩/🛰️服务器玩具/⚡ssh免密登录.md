# 0 本地操作

首先，在本地客户端生成公私钥（一直回车就行）
```shell
ssh-keygen
```

Windows生成的文件在用户目录下的 `.ssh` 文件夹里
```
1. id_rsa （私钥）
2. id_rsa.pub (公钥)
```

# 1 服务器端操作

将本地生成的公钥文件上传到服务器，然后写入到 `authorized_keys` 文件即可
```shell
cd ~/.ssh
cat id_rsa.pub >> authorized_keys
```
