# 怎样为你的 Git Commit 加上 GPG 的签名
> 以 ubuntu 为例

![截图](./screenshot.png)

注意 `git` 2.0 及以后的版本才支持 `GPG`

## 查看 git 版本

```
git --version
```

**确保 `git` 升级到 `2.x` 版本，如果需要可以执行下面的命令升级**

```sh
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get update
sudo apt-get install git
```

## 查看 GPG 是否安装

```
gpg --version
```

系统里一般都已经装上了 `GPG`，如果没有可以 **手动安装 GnuPG**

```
sudo apt-get install gnupg
```

## 生成 GPG Key

`gpg --gen-key`

**生成过程中需要键盘输入一些信息**

- 加密算法推荐选择 RSA
- 密匙长度推荐选择 4096
- 过期时间推荐选择 1y 也就是一年
- 名字
- 邮箱 需要是 Github 认证过的邮箱
- 注释

最后需要输入一个密码，类似于保护 `SSH` 证书的密码

接下来是漫长的生成过程，在等待的过程里可以多移动鼠标，打开几个网页，以此来增加随机性。

## 查看 GPG Key ID

```
gpg --list-kesy
```

```
~$ gpg --list-keys
/home/*/.gnupg/pubring.gpg
----------------------------
pub   4096R/6******7 2016-05-11 [expires: 2017-05-11]
uid                  Wang Yan <wyvernnot@gmail.com>
sub   4096R/B******E 2016-05-11 [expires: 2017-05-11]
```

输出结果里的 6******7 就是这个 `GPG Key ID`

## 导出 GPG 公匙

执行命令：

```
gpg --armor --export 6******7
```

会在命令行输出公匙的文本

```txt
-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: GnuPG v1

mQINBFczB1kBEAC3afc50p08I7knynnm4ea/8VZy42zGH/qrIWoInqT8HbgwLb9s
TrulTgLMbMTTOhl4zqFtTR52ZTwfLOtnV8tCM62ZNqO6z0nd0bFPA7HVp5t9z4a5
qBF3l3AZO5TFjn8gTsxatpVf5Ug9gmkEOCAejwSPFKNvfAun+WSLZ19LaFcorrj1
                          ...
cOyj7VNdmx1//BMp/5A6SEqJFkkLxebaJpcrv1dm1HvKGU8vty3JAdWgOjY5VVmn
nUaAr8D0uNquASkap4dki/zscK+tEJiH6HLnQGGtIf+fR2zoHAUfBvtTuUhq8w/Q
F0R/UVgc4Sg+Vz4gyYNJKO8BdTTTfvOZMDNFw4x70vXfhCZDm4QxgfUwRs//kkIh
Z3QylTf5xSk4
=kuFS
-----END PGP PUBLIC KEY BLOCK-----
```

## 登录 Github 添加这个公匙

入口在 `Settings` 下的 `SSH and GPG Keys` 下面

## 配置 Git 启用 GPG 签名

全局设置默认使用的 `GPG Key ID`

```
git config --global user.signingkey 6******7
```

如果需要每次提交都默认都开启 `GPG`

```
git config --global commit.gpgsign true
```

## 测试提交

```
git commit -S -m your commit message
```

执行 `git push` 后登录到 `Github`， 查看 `Commit Message` 的右上角是不是多了一个 `Verified` 图标

![截图](./screenshot.png)

## 参考文章

- http://www.linuxidc.com/Linux/2014-11/109972.htm
- https://help.github.com/articles/generating-a-new-gpg-key/
- https://help.github.com/articles/adding-a-new-gpg-key-to-your-github-account/
- https://help.github.com/articles/telling-git-about-your-gpg-key/
- https://help.github.com/articles/signing-commits-using-gpg/