# Github -> Gitee



> **使用此仓库模板页面 `Use this template` 自行创建仓库。**
>
> **不宜过高频率同步至Gitee，避免可能的各种风险。**



</br>



### 秘钥

在仓库settings添加以下两个secrets：

- REPO_TOKEN
- SSH_PRIVATE_KEY



#### 1、Github 添加 REPO_TOKEN

右上角个人图标 -> `Settings` -> `Developer Settings` -> `Personal access tokens` -> `Tokens` -> `Generate new token`，填写相应名称，及选项，并复制生成的`token`；

在仓库`Settings` -> `Secrets and variables` -> `New repository serect`，`Name`填写`REPO_TOKEN`，`Secret`粘贴上步生成的`token`;



#### 2、Github 添加 SSH_PRIVATE_KEY

首先，[生成SSH密钥](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux)（可点击此处参考）；

```shell
root@debian:~/.ssh# ssh-keygen -t rsa -b 4096 -C "xxx@xxxxx.com"   # xxx@xxxxx.com改为常用邮箱即可
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa
Your public key has been saved in /root/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:CF8xfU8FecvbdVDf/ZhXvMwqdlTlVhhNSnXZ4y+bUF0 xxx@xxxxx.com
The key's randomart image is:
+---[RSA 4096]----+
|        o.    +@@|
|         o. ..=*E|
|    .   .  . o+=%|
|     o o      *BB|
|      o S    oo+O|
|            o ooo|
|           o + + |
|          . o o  |
|                 |
+----[SHA256]-----+
```

将密钥打印出`cat ~/.ssh/id_rsa`，并复制粘贴给 SSH_PRIVATE_KEY

```shell
root@debian:~/.ssh# cat ~/.ssh/id_rsa
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAACFwAAAAdzc2gtcn
....
pZZzg3TmkHC4+SY9iXvdARvC05wNWshbgj2XVA/NwQqXT83aAeGXoLMrp65sHoW1/Wwwwz
2+hLQZFE4QyfAAAADXh4eEB4eHh4eC5jb20BAgMEBQ==
-----END OPENSSH PRIVATE KEY-----
```



#### 3、Gitee 添加 SSH公钥

将公钥打印出`cat ~/.ssh/id_rsa.pub`，并复制公钥

```shell
root@debian:~/.ssh# cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDBLsbAHgz3ob30/A7NlSXXs4bpSTxLI+TV3OLDhfvRItuDV0E796xnUsLXXyCQYyBjMDZtgJsFpQDT1q0BDbEgqOBkBz4t8zgizKeMgD+evSo4ndG/kdPm38lCCWwnuG/LMYXGq5C1UvLfoay/fuIhFPQHtAjmroJN16rsU7+ExibOEc4tADphfxvS/71gtpx1SA5130jgXnVzoK+PVJhQWHlG+mLERbG8NW8AUXR7joM5qqCRcAlgpQNh/6v4Usan2tb/bkFBHhtCUTNNv3OM65BqTX5oze5FFUArpTzTLDCnsVavlS8L3vJsVCOsRRkybV6QKruvvNxgITPP7J0ddcXNu5lQt/oh9J05pcN4z4NOey3lQplBWMoGkbtO7U09prc0/kQqxPICf2dvwOM0HPELbttDJ+VtIMpqrsQ7XnhVQEH5r/lBwZkjukRBCc409Jr8Dx9QOnc6PiAT0MjBATt2LyYtKBcr8YrWLf/Xy1T56DSVkMF8YXtUlQR2JWAv7NNhjRh3bZKRI9rYR7nFtwt10Zao2tTlApYXMAJtznWoVagyuckOIm4LgV97wBLUiJynOcoDb9bW5kNgQuwe8+/HsU2xIfswIR9m8Qm2RWFSifoBUJyntFZU61CtrBsbbIWnpw82pwqqnam3q0te2Wp4alDRgV4b8i29JFtslQ== xxx@xxxxx.com
```

在`gitee`主页右上角个人图标 -> `设置` -> `SSH公钥` -> `添加公钥`，粘贴打印出的`公钥`；



</br>



### 设置



#### 1、修改需要同步的Github源仓库、Gitee目标仓库；

多个仓库同时同步，按相同格式在后面追加；

```yaml
  matrix:
    include:
        # 注意替换为需要同步的 GitHub 源仓库地址
      - githubUserName: GithubUser1			# Github源仓库用户名
        githubRepoName: Repository1			# Github源仓库名称
        # 注意替换为你的 Gitee 目标仓库地址
        giteeUserName: GiteeUser1			  # Gitee目标仓库用户名
        giteeRepoName: GiteeRepo1			  # Gitee目标仓库名称
        # 同步多个仓库，请按要求追加即可
      - githubUserName: GithubUser2
        githubRepoName: GithubRepo2
        giteeUserName: GiteeUser2
        giteeRepoName: GiteeRepo2
```





  #### 2、开启定时同步，请取消下面的3个“#”

```yml
  #schedule:
   #- cron: 30 4-16/3 * * 1-5
   #- cron: 30 0-16/3 * * 0,6
```



</br>



### 说明

- `Actions`->`Caches`，保存着上游Github仓库，最新commit的hash值

  运行actions即可同步至Gitee仓库，同时会将Github源仓库最新提交的commit值保存至actions缓存；

- gitee仓库体积过大，会导致同步失败！

  - `gitee`仓库->`管理`->`清空仓库`，再重新同步
  - [gitee仓库体积过大，如何减小](https://help.gitee.com/repository/base/%E4%BB%93%E5%BA%93%E4%BD%93%E7%A7%AF%E8%BF%87%E5%A4%A7%EF%BC%8C%E5%A6%82%E4%BD%95%E5%87%8F%E5%B0%8F)
  - github仓库体积过大，也会最终导致此类错误；可网上搜索瘦身方案；

- gitee.com域名解析失败

  如因解析gitee.com失败(`ssh: Could not resolve hostname gitee.com: Try again`)导致同步Gitee仓库失败，可删除缓存、或在workflow启动强制同步，来进行同步。

- Gitee仓库自带Github同步至Gitee仓库功能，也可以尝试下。

