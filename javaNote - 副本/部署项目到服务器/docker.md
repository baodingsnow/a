# 1.必要准备



搭建服务器环境，主要是安装部署docker环境

采用离线安装方式                                  需要 finalshell   tar包( docker  redis  mysql)   redis mysqltar包从本地docker直接导出\





## finalshell

远程文件管理                                   直接下载

## 导出方法 

 ![E4AD734B-AB0C-49ad-A3C1-C955D2B8B140](E:\Sadnote\sad\picture\E4AD734B-AB0C-49ad-A3C1-C955D2B8B140.png)

粘贴 imageId

到保存tar包的文件夹下 cmd         

```squirrel
//执行命令
docker  save  imageId   >文件路径\tar包名称.tar
```

mysql  redis tar包到此准备好



一般可以从网上直接找  本地有的话比较方便

# 2.在服务器安装docker

在对应的服务器home处新建两个文件     installfile dockerimages         分别用来装docker的tar包     服务配置的tar包    （本地tar包直接拖到服务器对应的文件夹就好）

解压安装包

```shell
tar -zxvf docker-20.10.14.tgz
```

将docker 相关命令拷贝到 /usr/bin

```shell
cp docker/* /usr/bin/
```

docker注册成系统服务

```shell
vim /etc/systemd/system/docker.service
```

```shell
[unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target
 
[Service]
Type=notify
ExecStart=/usr/bin/dockerd --graph /home/docker
ExecReload=/bin/kill -s HUP $MAINPID
LimitNOFILE=infinity
LimitNPROC=infinity
TimeoutStartSec=0
Delegate=yes
KillMode=process
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s
 
[Install]
WantedBy=multi-user.target
```

添加执行权限

```shell
chmod +x /etc/systemd/system/docker.service
```

启动服务

```shell
systemctl start docker
```

设置开机自启

```shell
systemctl enable docker.service
```

查看Docker版本

```shell
docker -v
```

## 启动数据库

```shell
docker run -d --network host --name dragon  -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:8.0.29 --lower_case_table_names=1
```



# 3.在服务器安装服务配置

此时tar包已保存在 dockerimages  

进入 dockerimages  路径

导出redis即可

```shell
 docker load < redis.tar
```

==ini只读//==

```
   #docker常用命令:
   
   docker ps  展示正在运行的数据库    docker  ps  -a  展示所有数据库
   docker  port  容器name   查看端口
 
1. 查看容器: docker ps
 
2. 查看镜像: docker images
 
3. 删除容器: docker rm 容器name
 
4. 删除镜像: docker rmi 镜像id
 
5. 创建容器: docker run --name dockermysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=654321 mysql:5.7.23
 
6. 启动容器: docker start 容器name #docker start dockermysql
 
7. 重启容器: docker restart dockermysql
 
8. 停止容器: docker stop dockermysql
 
9. 容器交互: docker exec -it dockermysql bash #或 docker attach dockermysql
 
10.退出交互: Ctrl+P,Ctrl+Q(Ctrl键一直保持按下)
 
11.设置开机自启: systemctl enable docker
 
12.容器设置自启,update命令: docker update --restart=always aeccnginx
```

```
linux常用命令
```



```shell
docker run --name mysql -p 3306:3306 -v $PWD/conf/my.cnf:/etc/mysql/my.cnf  -v  $PWD/logs:/logs -v $PWD/data:/mysql_data -e MYSQL_ROOT_PASSWORD=1qaz@wsx   -d mysql:8.0.29

docker run -p 3306:3306 --name mysql -v $PWD/conf/my.cnf:/etc/mysql/my.cnf -v $PWD/logs:/logs -v $PWD/data:/mysql_data -e MYSQL_ROOT_PASSWORD=123456 -d mysql

docker run -d  --privileged=true -p 6379:6379 -v /home/redis/redis.conf:/etc/redis/redis.conf -v /home/redis/data:/data --name redis redis:5.0.4 redis-server /etc/redis/redis.conf --requirepass "1qaz@wsx" --appendonly yes
```

