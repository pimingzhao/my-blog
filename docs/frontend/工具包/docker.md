# Docker

## 容器数据卷
匿名和具名挂载，指定路径挂载
### 指定路径挂载
```shell
docker run -v /home/test:/etc/ngix --name nginx -d nginx
```
### 具名挂载
```shell
# 默认会挂载到本地docker服务下的路径中
# 具体可以通过 docker inspect 查看详细信息
docker run -v test:/etc/ngix --name nginx -d nginx
```
### 匿名挂载
```shell
docker run -v /etc/ngix --name nginx -d nginxDockerFile
```

## 指令
```shell
FROM  # 基础镜像
WAINTAINER # 镜像是谁写的，姓名+邮箱 pimzh<2251@qq.com>
RUN   # 执行哪些命令
WORKDIR # 指定容器启动的工作目录 docker run -it 时默认进入的目录
CMD  # 指定容器启动的时候要运行的命名，只有最后一个会生效，可被替代
ENTERPOINT # 指定容器启动的时候要运行的命名，可以追加命令
ONBUILD # 
COPY # 将我们的文件拷贝到镜像中
ENV # 构建的时候设置环境变量通过 docker history imagesId 可以查看镜像的制作工作
docker build -f DOCKERFILE文件 -t 镜像名:版本 . # CMD 与 ENTERPOINT的区别
```
### CMD
```shell
# 1.  添加配置文件
vi testfile
​
FROM centos
CMD ["ls", "-a"]
​
# 2. 制作镜像
docker build -f testfile -t cmdtest .

# 3. 生成、启动容器
docker run dd823f34

# 4. 容器执行ls命令
ls -a
# .
# bin
# etc
# ...

# 5. 追加命令
docker run dd823f34 l

# 6. 报错
# error...

```

##​# ENTERPOINT

```shell
# 1.  添加配置文件
vi test2file
​
FROM centos
ENTERPOINT ["ls", "-a"]

# 2. 制作镜像
docker build -f test2ile -t cmdtest .

# 3. 生成、启动容器
docker run ada32fsd

# 4. 容器执行 ls -a 命令
# .
# bin
# etc
# ...

# 5. 追加命令
docker run dd823f34 l

# 6. 正常执行
# bin etc ...
```

## Dockerfile
编写dockerfile文件，官方命名 Dockerfile ，使用这个命名，我们可以在 build 的时候不用指定 -f 了，它会自动寻找