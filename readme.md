# 运行
```shell
$ docker-compose up -d
```

# nginx代理

```shell
server {
    listen 80;
    server_name git.chudeer.com;
    location / {

        proxy_set_header    Host                $http_host;
        proxy_set_header    X-Real-IP           $remote_addr;
        proxy_set_header    X-Forwarded-For     $proxy_add_x_forwarded_for;
        proxy_set_header    X-Forwarded-Proto   $scheme;

        proxy_pass http://127.0.0.1:5000/;
    }
}
```

# 配置gitlab
进入容器: 
```shell
$ docker-compose exec gitlab bash
```
编辑/etc/gitlab/gitlab.rb，修改下面3个项目：
```
external_url 'http://git.chudeer.com'
gitlab_rails['gitlab_ssh_host'] = 'git.chudeer.com'
gitlab_rails['time_zone'] = 'PRC'
```
使配置生效
```shell
$ gitlab-ctl reconfigure
```

浏览器打开 http://git.chudeer.com，设置初始密码，然后用root账号登录。
