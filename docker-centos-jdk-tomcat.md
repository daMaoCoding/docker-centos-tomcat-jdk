# 本文目标

> 实现基于最新的centos镜像上，构建jdk8+tomcat9镜像，mysql镜像，运行war包，并一起做成镜像，实现一次构建随处可行。我本地是mac的，windows尚未实践。

* 具体步骤:

> 打开mac终端，以下命令均在终端执行。前提是要先安装了docker服务。这个可以谷歌或者百度自行安装了。

> docker images
>
> docker search centos 
>
> 准备好jdk 和 tomcat ，可以到官网下载压缩包到本地。
>
> jdk地址:```https://www.oracle.com/java/technologies/javase-jdk8-downloads.html```
>
> tomcat:```https://tomcat.apache.org/download-90.cgi```
>
> 如果本地使用的版本正好适合使用那么就不必下载了，直接使用本地的安装包即可，只需复制一份。
>
> 从docker 仓库拉最新的centos:
>
> docker pull centos 
>
> 看网络速度咯。
>
> ...
>
> docker images 
>
> 进入 centos
>
> docker run -i -t  centos:latest /bin/bash
>
> 常用命令：
>
> docker ps 
>
> docker ps -a 
>
> 进入容器命令2条都可以
>
> docker exec -it 容器id /bin/bash
>
> docker attach 容器id
>
> 
>
> docker stop 容器id或者名称
>
> docker start 容器id或者名称
>
> docker rm 容器id或者名称(先停止再删除)
>
> 退出容器: exit;



* 继续

  >mkdir docker-test
  >
  >cd docker-test
  >
  >拷贝 jdk tomcat 到
  >
  >cp /Users/leochow/Downloads/apache-tomcat-9.0.31.tar.gz  ./
  >
  >cp 
  >
  >/Users/leochow/Downloads/jdk-8u241-linux-x64.tar.gz  ./
  >
  >tar -zxvf xxxx
  >
  >rm -rf xxxx.tar.gz 
  >
  >在当前文件夹下：
  >
  >touch Dockerfile
  >
  >touch env.sh
  >
  >touch initdocker
  >
  >mkdir 项目名称比如java的项目
  >
  >
  >编辑：
  >
  >vi env.sh
  >
  >export JAVA_HOME=/usr/local/jdk
  >
  >export CLASSPATH=$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
  >
  >export CATALINA_HOME=/usr/local/tomcat
  >
  >export CATALINA_BASE=/usr/local/tomcat
  >
  >export PATH=$PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
  >
  >
  >
  >编辑 initdocker
  >
  >\#!/bin/bash
  >
  >
  >
  >. /etc/profile
  >
  >
  >
  >\#start ssh
  >
  >/usr/sbin/sshd
  >
  >
  >
  >\#start tomcat
  >
  >/usr/local/tomcat/bin/catalina.sh start
  >
  >
  >
  >echo "SUCCESS HJ" >>/var/log/message
  >
  >
  >
  >tail -f /var/log/message
  >
  >
  >
  >编辑Dockerfile
  >
  >FROM centos:latest
  >
  >MAINTAINER chow email 787021894@qq.com
  >
  >ADD jdk1.8.0_241 /usr/local/jdk
  >
  >ADD apache-tomcat-9.0.31 /usr/local/tomcat
  >
  >\## 这个是项目war包 要修改
  >
  >ADD xincheng /usr/local/tomcat/webapps/xincheng
  >
  >ADD env.sh /etc/profile.d
  >
  >ADD initdocker /sbin/
  >
  >CMD ["/sbin/initdocker"]
  >
  >构建镜像
  >docker build -t 项目-中间件:版本号  . 【记住最后的这个点号】
  >
  >如果构建失败，可能是那几个文件的权限不够，修改执行权限：chmod +x xx.sh 
  >然后 删除 镜像，如果已经运行了容器，先停止再删除。
  >
  >docker ps -a 查看容器
  >
  >docker stop 容器ID或者名称
  >
  >docker rm 容器id或者名称
  >
  >docker images 查看所有镜像
  >
  >docker rmi 镜像id或者名称
  >
  >
  >重新构建。然后运行
  >
  >Docker run -d -p 18080:8080 镜像名称：版本
  >
  >看看是否运行成功。
  >
  >docker ps -a 
  >
  >localhost:18080 看看是否能访问tomcat的首页。
  >
  >要部署项目的话，数据库是要部署的，可以制作镜像或者放在本地。然后配置容器调用数据库端口。
  >
  >
  >
  >镜像上传导出
  >
  >docker save -o xxx.tar 镜像id 可以将镜像保存为一个tar包;保存之后可以在任何地方加载镜像了，将生成的tar文件上传（上传命令 自行百度）到需要加载镜像的服务器后可以使用docker load :
  >
  >docker load -i xxxxx.tar 将tar导成镜像；之后可以使用docker images查看

  

   

  

  

  

  

  

  

  >
  >
  >

  











