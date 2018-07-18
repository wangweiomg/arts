## ARTS - Tip

### ssh 登录配置

在登录远程服务器时候，很多是用ssh-key来验证的，每次要输入ip地址会比教麻烦，于是就产生了配置

在 ~/.ssh/ 目录创建config文件，配置如下：

```
Host www
    HostName 182.159.15.88
    Port 50
    User root
    IdentityFile ~/.ssh/id_rsa_2048

# git指定私钥
Host api
    HostName api.domain.com
    User git
    Port 22
    IdentityFile ~/.ssh/git_id_rsa


```

这样登陆时候直接  ssh www 就可以了， 还有在不操作一段时间后会自动断开，需要再设置一个：

```
 Host *
 ServerAliveInterval 60
 #表示没60秒去给服务端发起一次请求消息
```