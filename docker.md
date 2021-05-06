## docker 

(参考博客)[https://blog.csdn.net/u013378306/article/details/86668313]

#### 根据dockerfile构建镜像

``` docker
    <!-- -t 重命名镜像名称 . dockerfile配置文件路径 -->
    docker build -t myweb:1.0 .    
```

#### 启动镜像创建容器

``` docker
    <!-- -p 80:3000 映射的端口:镜像的端口 IMAGE ID 镜像的id或者名称-->
    docker run -p 80:3000  IMAGE ID
```

#### 查看启动的镜像

``` docker
    docker ps
    docker ps -a
```

#### 查看已创建的镜像

``` docker
    docker images
```

#### 停止正在启动的镜像

``` docker
    docker stop :id
```

#### 删除镜像

``` docker
    docker rmi IMAGE ID
    <!-- 强制删除 -->
    docker rmi -f IMAGE ID

```

#### 删除容器

``` docker
    docker rm :id

```

#### 拉取镜像

``` docker
    docker pull xxx
```

#### 查看容器日志

``` docker

    docker logs :id
```
