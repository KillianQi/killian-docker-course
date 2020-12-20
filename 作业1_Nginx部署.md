# 作业一 ：Nginx部署

**1.搜索镜像**
```

# 搜索镜像 分别使用命令和docker hub页面搜索，并查看帮助文档

[root@killiandocker01 home]# docker search nginx
NAME                               DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                              Official build of Nginx.                        14172     [OK]
jwilder/nginx-proxy                Automated Nginx reverse proxy for docker con…   1929                 [OK]
richarvey/nginx-php-fpm            Container running Nginx + PHP-FPM capable of…   797                  [OK]
linuxserver/nginx                  An Nginx container, brought to you by LinuxS…   135
jc21/nginx-proxy-manager           Docker container for managing Nginx proxy ho…   121

```

![20201220203706](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220203706.png)

网页地址：https://hub.docker.com/_/nginx


**2.拉取镜像**
```

# 下载镜像 docker pull nginx

[root@killiandocker01 home]# docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
6ec7b7d162b2: Pull complete
cb420a90068e: Pull complete
2766c0bf2b07: Pull complete
e05167b6a99d: Pull complete
70ac9d795e79: Pull complete
Digest: sha256:4cf620a5c81390ee209398ecc18e5fb9dd0f5155cd82adcbae532fec94006fb9
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest

```


**3.运行测试**

```
# 查看拉取的镜像，并运行
[root@killiandocker01 home]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    ae2feff98a0c   4 days ago    133MB
centos       latest    300e315adb2f   12 days ago   209MB

# 执行docker run启动容器
    -d为后台运行 
    --name为命名 
    -p为指定暴露端口
[root@killiandocker01 home]# docker run -d --name nginx01 -p 3344:80 nginx  #创建容器，映射宿主机的3344端口为容器的80端口，并命名为nginx01  
1c13b3dfb2b8b06e22d73aa8af96c1eb2cbfdb48a090f602eae098379148f907
```

```
# 测试刚刚启动的nginx

[root@killiandocker01 home]# curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
[root@killiandocker01 home]#


```

打开网页测试查看

![20201220204800](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220204800.png)


端口暴露的概念

![20201220204647](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220204647.png)


**4.进入容器并修改相关参数查看变化**

```

[root@killiandocker01 home]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                  NAMES
1c13b3dfb2b8   nginx     "/docker-entrypoint.…"   10 minutes ago   Up 10 minutes   0.0.0.0:3344->80/tcp   nginx01
fbee06ce2532   centos    "/bin/bash"              5 hours ago      Up 5 hours                             musing_sutherland
[root@killiandocker01 home]# docker exec -it 1c13b3dfb2b8 /bin/bash
root@1c13b3dfb2b8:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@1c13b3dfb2b8:/# cd /etc/nginx
root@1c13b3dfb2b8:/etc/nginx# ls
conf.d  fastcgi_params  koi-utf  koi-win  mime.types  modules  nginx.conf  scgi_params  uwsgi_params  win-utf
root@1c13b3dfb2b8:/etc/nginx#

```


***思考题：***

***创建容器完成后，发现每次修改配置文件，都需要进入容器内部进行修改。是否可以通过别的方式，在容器外直接修改配置文件，使配置生效？***