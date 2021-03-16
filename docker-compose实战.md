# 一步一步docker-compose实战经验

## 实践一： django+mysql

> 参照的操作指南地址： https://docs.docker.com/compose/django/



```
使用docker-compose构建一个 django、mysql的镜像
```

### 操作步骤

- 1、创建一个空的项目结构，名称叫做 compose-project, 这里面，我们如果需要jdk的jar包、tomcat的jar、python安装包，都放进这个文件夹

![image-20201011094311083](F:\MD格式学习笔记库\image-20201011094311083.png)

- 2、在项目目录新建一个<kbd>DockerFile</kbd>的文件

> 这个文件放镜像的构建命令，官方介绍指南:[ [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)]https://docs.docker.com/engine/reference/builder/

![image-20201011094515027](F:\MD格式学习笔记库\image-20201011094515027.png)

文件名字要是Dockerfile

- 3、在Dockerfile里面写入如下的内容

```dockerfile
FROM python:3
ENV PYTHONNOBUFFERED=1
RUN mkdir /webapps
WORKDIR /webapps
COPY requirements.txt /webapps/
RUN pip install -U pip && pip install -r requirements.txt -i https://pypi.douban.com/simple
COPY . /webapps/
```

![image-20201011095559432](F:\MD格式学习笔记库\image-20201011095559432.png)



- 4、在项目目录下，新建requirements.txt，并添加如下内容

```
Django>=2.2
mysqlclient
uwsgi
```

![image-20201011100106424](F:\MD格式学习笔记库\image-20201011100106424.png)

- 5、在项目目录下，新建一个docker-compose.yml文件，这个是compose的构建文件，添加如下内容

```yaml
version: "3"

services:
  db:
    image: mysql:5.6
    environment:
      - MYSQL_DATABASE=docker-compose
      - MYSQL_USER=root
      - MYSQL_PASSWORD=123456
      - TZ=Asia/Shanghai

  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8080
    volumes:
      - .:/webapps
    ports:
      - "8000:8000"
    depends_on:
      - db
```

- 6、进入到项目根目录，执行构建django项目的过程

```shell
docker-compose run web django-admin startproject composeexample .

# 指令解释
  docker-compose run web : 在当前目录查找docker-compose.yml，并运行单独web服务，如果没有这个服务，就会进行构建
  django-admin startproject composeexample: 当web服务创建成功并启动container后，执行这条命令
```

具体执行效果如下：

![image-20201011102416654](F:\MD格式学习笔记库\image-20201011102416654.png)

待项目构建成功后，创建了两个镜像 ，并执行命令

![image-20201011102540239](F:\MD格式学习笔记库\image-20201011102540239.png)

> <font color="red">备注</font>: 
>
> 如果宿主机是Linux系统， 需要对django项目进行授权   sudo chown -R $USER:$USER .
>
> 如果宿主机是Mac系统或者Windows系统，文件会自动已经具备了授权     

- 7 在项目中配置我们的数据库连接

![image-20201011103411807](F:\MD格式学习笔记库\image-20201011103411807.png)



可以看看我们的docker-compose.yml 的配置和数据库这个一一对应

![image-20201011103455033](F:\MD格式学习笔记库\image-20201011103455033.png)

- 8 进入到我们的根目录，然后执行 docker-compose up  

![image-20201011103618672](F:\MD格式学习笔记库\image-20201011103618672.png)



<font color='red'>使用过程遇到的坑:</font>

1.  执行docker-compose up，一直无法构建成功，出现的错误情况有两种现象，一种是无法构建成功web应用，一种是构建成功web应用但是无法启动成功，显示数据库启动，但是web不启动

   错误原因: docker desktop在windows上，如果yaml有挂载的参数，需要在docker的设置里面，把目录的File share权限开放出来，如下图：

   ![image-20201012085939968](F:\MD格式学习笔记库\image-20201012085939968.png)



> 问题1：我的项目代码在d:\src下，所以我把d:\src设置了File Sharing
>
> 找这个问题花了一天时间，先尝试吧yaml服务简化到只有django,后来跑官方例子，都不行，最后重新安装了docker软件，新版本给了报错信息，才查到错误原因。
>
> 问题2： 执行docker-compose run web django-admin.py startproject achievements ./achievements  可以执行成功，但是数据卷没有同步
>
> 错误原因： 因为volumes 里面写错了
>
>                    - . /webapps/   正确的应该是 - . :/webapps/
>
> 问题3：安装官方的例子，使用pgsql，一直无法通过
>
>   我后来卸载掉了原来的Docker，安装了最新版本的Docker，解决了这个问题



<font color='red'>随意几个知识点：</font>

1. Docker删除所有容器：

   docker rm `docker ps -a -q`

   最重要的是后面的 -q选项，表示只显示ID。

2. 删除none镜像：

   docker rmi `docker images -f "dangling=true" -q`

3. 更新。dockerfile要加上apt-get update，否则后面的命令不能正常执行；

4. command命令只执行最后一个，在脚本中写了三个命令，但最后只执行最后一个。后来把三个命令用 && 拼接起来。





## 实践二： :docker+mysql+nginx+gunicorn

> 上面的步骤实现了，中间踩了不少的坑，如上述文件所述，但最终还是调通了，接下来实现更加生产化的环境，docker+mysql+nginx+gunicorn
>
> 难点：
>
> 1、如何使用了nginx并映射静态文件，
>
> 2、如何在docker-compose up执行项目的 python manage.py migrate 以及启动django,如何将自己的配置文件
>
> 3、如何实现服务之间的依赖
>
> 4、docker-compose.yaml里面的路径到底是怎么回事

### 网络架构

感谢hbnn111大牛，套用博主[hbnn111Docker部署Web应用__Django](https://blog.csdn.net/hbnn111/article/details/77176629?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param) 的图片，我就不画了，这个博主使用的Docker应该是较早的版本。

![image-20201013121838224](F:\MD格式学习笔记库\image-20201013121838224.png)

通过上图，大致了解需要构建的容器： Nginx容器，WebServer容器，MySQL容器，Memcached容器，Redis容器

### 准备的环境

宿主机: windown 10 , Docker:  19.03.12  + 4.2.2

![image-20201013122757671](F:\MD格式学习笔记库\image-20201013122757671.png)

1、在宿主机上创建一个空文件夹，名字自己起，我的名字起的是Docker_Project，先建立如下的工程结构

![image-20201013221129775](F:\MD格式学习笔记库\image-20201013221129775.png)

2、在宿主机，进入到docker_demo，执行 django-admin startproject dockerblog

![image-20201013221236768](F:\MD格式学习笔记库\image-20201013221236768.png)

3、在宿主机，进入到刚才创建的django项目目录，创建两个app

![image-20201013221332111](F:\MD格式学习笔记库\image-20201013221332111.png)

此时工程结构如下：

![image-20201013221404856](F:\MD格式学习笔记库\image-20201013221404856.png)

4、在D:\src\docker_demo\dockerblog\目录下，添加media文件夹，static文件夹，Dockerfile,requirements.txt,start.sh

![image-20201013221625532](F:\MD格式学习笔记库\image-20201013221625532.png)

```shell
说明:
1、dockerblog 是django项目的工程目录， 里面的Dockerfile为接下来我们要进行build的文件，requirements.tx是django的依赖包，start.sh是我们up项目时，将执行的初始脚本
static是静态文件的存放目录
media是 静态多媒体文件的存放目录
2、在nginx中， 是nginx的服务构建文件，含有Dockerfile和nginx.conf
Dockerfile编排了nginx相关的脚本
nginx.conf 录入了nginx的相关服务配置
3、docker-compose.yml 构建镜像和容器的配置脚本
```

接下来，我们需要分别取编辑dockerblog的Dockerfile, start.sh,requirements.txt ,nginx的Dockerfile, nginx.conf以及docker-compose.yml

### 编排配置文件

#### 编排django应用配置

> 1）Dockerfile

```docker
FROM python:3.6-stretch

ENV PYTHONUNBUFFERED=1

RUN mkdir /webapps

WORKDIR /webapps

COPY requirements.txt /webapps/

RUN pip3 install -r requirements.txt -i https://pypi.douban.com/simple

EXPOSE 80 8000

COPY . /webapps/
```

基础镜像我使用了 python:3.6-stretch, 设置了pythonunbuffered=1, 

同时，在将要构建的容器内，创建一个webapps的文件目录，并将当前的工作目录设置为webapps

从宿主机当前路径(这里面的目录是指相对于执行的Dockerfile的路径)的requirements.txt拷贝到容器的webapps目录下

在容器中执行 pip3 install -r requirements.txt -i https://pypi.douban.com/simple ,安装对应的pip包

对外开放80 ，8000两个端口，执行完后，将宿主机当前路径下的所有内容拷贝到容器的webapps下，这里.代表是拷贝和Dockerfile同目录下的所有文件，也就是Docker_project\dockerblog\目录下所有文件拷贝到\webapps\下面

> 2） 启动脚本start.sh

```shell
#!/bin/bash

#命令只执行最后一个，所以使用 &&
python manage.py collectstatic --noinput &&
python manage.py migrate &&
gunicorn dockerblog.wsgi:application -w 2 -k gthread -b 0.0.0.0:8000 --log-level DEBUG --chdir=/webapps
```

当项目初次部署，需要手机各个app的static目录到工程的static目录下，同时要创建数据库，上面三个命令是通过 &&拼接，相当于执行一个命令，此外 django的应用选择gunicorn做web服务器，对应的指令意思如下：

-w 2: 启动2个进程

-k gthread : 使用的工作模式，选择gthread

-b 0.0.0.0:8000 :绑定监听的地址和端口

-chdir=/webapps 执行这个命令之前，先进入到/webapp目录下

--log-level debug :输出的日志级别

--timeout 60 : 

--max_requests 5000 worker重启之前处理的最大requests数量，主要防止内存溢出

> 3)  安装依赖包配置 requirements.txt

```

bcrypt==3.2.0
cffi==1.14.2
coverage==5.2.1
cryptography==3.1
Django==2.2.14
django-filter==2.3.0
django-haystack==2.8.1
django-mdeditor==0.1.18
django-pure-pagination==0.3.0
djangorestframework==3.11.1
fabric==2.5.0
Faker==4.1.2
gunicorn==20.0.4
importlib-metadata==1.7.0
invoke==1.4.1
Markdown==3.2.2
mysqlclient==2.0.1
paramiko==2.7.2
Pillow==7.2.0
pycparser==2.20
Pygments==2.7.1
PyNaCl==1.4.0
python-dateutil==2.8.1
pytz==2020.1
six==1.15.0
sqlparse==0.3.1
text-unidecode==1.3
zipp==3.1.0
```

这个可以根据自己需要删减，因为之前项目这个，所以没动，就直接用了

#### 编排Nginx配置

> 1)  编辑Dockerfile

```dockerfile
FROM nginx

RUN rm /etc/nginx/conf.d/default.conf

COPY nginx.conf /etc/nginx/conf.d/

RUN mkdir -p /usr/share/nginx/html/static

RUN mkdir -p /usr/share/nginx/html/media

EXPOSE 80 8000
```

nginx的基础镜像选择docker仓库，删除默认的 /etc/nginx/conf.d/default.conf，将和Dockerfile同一层级文件nginx.conf拷贝到 /etc/nginx/conf.d/,   接下来添加static和media，作为web应用的静态文件存储

> 2) nginx.conf

```
server{
	listen 80;
	server_name localhost;
	charset utf-8;

	error_log /tmp/nginx_error.log;
	access_log /tmp/nginx_access.log;

	location /media {
		alias /usr/share/nginx/html/media;
	}
	location /static {
		alias /usr/share/nginx/html/static;
	}
	location / {
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header Host $http_host;
		proxy_redirect off;
		proxy_pass http://web:8000;
	}
}
```

关于Nginx配置，有两点说明，非常重要:

1、location  静态文件配置，nginx指定的静态文件原目录实在/usr/share/nginx/html/，而该目录下的静态文件是从web容器中通过volumes同步的。 所以接下来的docker-compose是非常重要的。

2、proxy_pass 这和你直接在主机上配置是不一样的，host不能写成具体ip，要写服务名，这里要写web service 的name, web是在docker-compose中定义的web应用的service名称。 后面要在docker-compose.yml里面配置

#### docker-compose.yml配置

```yaml
version: "3.8"

services:
  db:
    image: mysql:5.6
    environment:
      - MYSQL_DATABASE=app_blog
      - MYSQL_ROOT_PASSWORD=123456
    volumes:
      - ./srv/db:/var/lib/mysql
    expose:
      - "3306"
  
  redis:
    image: redis

  memcached:
    image: memcached

  web:
    build: ./dockerblog
    volumes:
      - ./dockerblog:/webapps
      - logvolume01:/tmp
    command: bash start.sh
    depends_on:
      - db

  nginx:
    build: ./nginx
    ports:
      - "80:80"
    volumes:
      - ./dockerblog/static:/usr/share/nginx/html/static:ro
      - ./dockerblog/media:/usr/share/nginx/html/media:ro
    depends_on:
      - web
    restart: always
volumes:
  logvolume01: {}
```

分别解释下这几个服务

> 1、db，Mysql数据库，配置了如下几个方面

- 从docker仓库下载mysql:5.6基础镜像
- 配置环境变量，创建一个数据库(数据库名称为blog_backend，用户名为root,密码是123456, 接下来在django的setting里面将配置这些信息，数据库执行migrate操作时将会用到)
- volumes 数据卷，为了实现备份,挂载数据卷，宿主机的目录为./db，/var/lib/mysql是mysql容器内目录，这个需要在主机上创建一个目录

关于compose的数据卷指定路径格式可以是下面多种形式

```shell
volumes:
  # 只是指定一个路径，Docker 会自动在创建一个数据卷（这个路径是容器内部的）。
  - /var/lib/mysql

  # 使用绝对路径挂载数据卷
  - /opt/data:/var/lib/mysql

  # 以 Compose 配置文件为中心的相对路径作为数据卷挂载到容器。
  - ./cache:/tmp/cache

  # 使用用户的相对路径（~/ 表示的目录是 /home/<用户目录>/ 或者 /root/）。
  - ~/configs:/etc/configs/:ro

  # 已经存在的命名的数据卷。
  - datavolume:/var/lib/mysql
```

- expose 指定对外暴露的端口
- restart 默认是no,意思在任何情况下都不会重启，如果设成always,就是如果stop，就会重启；
- root用户的密码; 你在django应用的settings.py里面也要写成响应的配置，具体如下:

```shell
DATABASES = {
'default': {
    'ENGINE': 'django.db.backends.mysql',
    'NAME': 'app_blog',
    'USER': 'root',
    'PASSWORD':'123456',
    'PORT':3306,
    'HOST':'db',
}
}
```

> 2、redis，memcached配置

这两个直接从仓库镜像拉取，不需要重新配置、

> 3、web应用

配置的几个方面

- build . 根据Dockerfile重新build一个镜像；
- ports 格式为HOST:CONTAINER, HOST为宿主机的端口，CONTAINER为容纳器端口。8000:8000代表宿主机8000映射容器内部的8000
- volumes 设置数据文件备份，也可以说成是同步，宿主机/dockerblog目录下内容同步web容器的webapps下，这是一个双向的同步，宿主机/tem/logs目录下的内容同步到web容器的/tmp下；
- command bash start.sh  执行容器上面工作目录下的start.sh文件
- depends_on: 他有两层含义，一层是在启动服务的时候，会先启动db,然后再启动web，而是如果执行docker-compose up web,也会创建和启动db

> 4、nginx配置

- build ： 根据Dockerfile重新build一个镜像；
- ports : 格式为HOST:CONTAINER。相当于一个nat转换，设置内部的端口向外转发的端口； http默认端口
- volumes： 设置数据同步，宿主机的./dockerblog/static同步容器的/usr/share/nginx/html/static ，ro代表只读模式，只能在宿主机更改， 宿主机的./dockerblog/media同步容器的/usr/share/nginx/html/media
- depends_on: 同上面

关于Nginx能够实现静态文件的映射、同步有个逻辑

 首先:我们在容器执行python manage.py collectstatic, 会将静态文件收集到 /docker_blog/static目录下；

然后，因为我们在web的Dockerfile设置了 volumes: - ./docker_blog:/webapps，所以容器的static内容会同步到宿主机的/docker_blog/static目录下

最后，我们在这里设置

```
volumes:
        - ./dockerblog/static:/usr/share/nginx/html/static:ro
        - ./dockerblog/media:/usr/share/nginx/html/media:ro
```

这样当宿主机的响应文件夹发生变动，又会同步到nginx容器的/usr/share/nginx/html/static和/usr/share/nginx/html/media文件下。

到目前为止，所有的部署相关的配置都已经做完了，

### 执行

![image-20201013211336368](F:\MD格式学习笔记库\image-20201013211336368.png)

最后执行成功

![image-20201013211355809](F:\MD格式学习笔记库\image-20201013211355809.png)

### 碰到的坑

1、未设置 ALLOWED_HOSTS = ['*']，导致无法访问

2、STATIC_URL = '/static/'
STATICFILES_DIRS = [os.path.join(BASE_DIR, "static")]
"""部署的时候执行python manage.py collectstatic，django会把所有App下的static文件都复制到STATIC_ROOT文件夹下"""
STATIC_ROOT = os.path.join(BASE_DIR, "/static/")

3、       | ModuleNotFoundError: No module named 'dockerblog.wsgi.application'; 'dockerblog.wsgi' is not a package

错误的使用形式: dockerblog.wsgi.application   正确形式：dockerblog.wsgi:application

gunicorn dockerblog.wsgi.application -w 2 -k gthread -b 0.0.0.0:8000 --log-level DEBUG --timeout 30 --chdir=/webapps 

4、OSError: [Errno 38] Function not implemented

```yaml
# 使用这个配置就出上述错误
  web:
    build: ./testblog
    volumes:
      - ./testblog:/webapps
      - ./tmp:/tmp
    command: bash start.sh
    depends_on:
      - db
#改成下面的配置就可以正常运行
services:
  web:
    build: ./testblog
    volumes:
      - ./testblog:/webapps
      - logvolume01:/tmp
    command: bash start.sh
    depends_on:
      - db

volumes:
  logvolume01: {}
```

5、我在docker-compose.yml的db服务里面定义数据库为appblog, 但是进入数据库，却生成了app_blog,导致数据库连接失败

```yaml
  #出错的配置
   db:
    image: mysql:5.6
    environment:
      - MYSQL_DATABASE=appblog
      - MYSQL_ROOT_PASSWORD=123456
    volumes:
      - ./srv/db:/var/lib/mysql
    expose:
      - "3306"
  # 修改后的配置
  db:
    image: mysql:5.6
    environment:
      - MYSQL_DATABASE=app_blog
      - MYSQL_ROOT_PASSWORD=123456
    volumes:
      - ./srv/db:/var/lib/mysql
    expose:
      - "3306"
```

## 实践三: 已存在项目使用docker

### 项目背景介绍

​	至此，我从最简单的django+mysql，执行到django+mysql+nginx+gunicorn的项目，前面两个项目虽然碰到了各种各样的坑，但基本都解决了，填坑的过程中，对docker的理解也多了一些，接下来，我将在自己已经存在的项目中使用docker.

​	接下来的docker文件结构和上述两个项目结构有些不同，因为docker是后加入的，所以项目结构如下：

![image-20201014094628534](F:\MD格式学习笔记库\image-20201014094628534.png)

项目分析：

在这个项目中，首先使用Pycharm编辑器，所以有.idea文件夹，使用了vscode编辑器，所以有.vscode，工程的app放在apps里面，工程的启动入口在blogproject, media是上传多媒体目录，uploads是上传路径目录，statci是服务器运行静态文件目录，scripts是fake构造虚拟数据的地方

_credentials.py 里面存放着敏感的github数据_

fabfile.py  是实现git项目自动同步数据

requirements.txt存放pip 安装文件列表

> 通过上述分析，这个工程中，对于一些敏感文件我们就不需要进行同步到服务器， 所以我们编辑了一个.dockerignore文件



### 准备环境

项目环境 python3.7 django =2.2   mysql =5.6 nginx,gunicorn. 通过上述分析，结合前面的知识，初步了解我们需要的构建步骤如下

1、在工程目录下，增加docker-compose.yml的 配置文件

2、需要构建python3.7服务，这儿需要一个Dockerfile，需要有启动start.sh，自动执行数据库迁移、静态文件收集、服务启动

3、在python3.7 服务上构建web服务，安装django,gunicorn

4、需要构建nginx服务，添加nginx的配置

5、需要做数据卷的部分含：

			- 对于数据库要做数据卷；
			- 对于项目工程，因为都存在变化，要做数据卷，同时过滤掉部分内容；
			- 对于nginx要做数据卷，同步项目的静态文件

通过上述分析，其实和我们之前结构类似，但也有不同的地方，我们在这个项目，将单独把所有的Dockerfile统一放到一个文件夹进行管理，管理结构如下

![image-20201014095924423](F:\MD格式学习笔记库\image-20201014095924423.png)

增加两套环境，这样进行切换更加便利

### 编排配置文件

编排后的项目目录结构

![image-20201014101100739](F:\MD格式学习笔记库\image-20201014101100739.png)

#### 编排django配置文件

> 1 ) Dockerfile配置

```dockerfile
FROM python:3.6-alpine

ENV PYTHONUNBUFFERED 1

ENV APP /webapps

RUN apk update \
  # Pillow dependencies
  && apk add jpeg-dev zlib-dev freetype-dev lcms2-dev openjpeg-dev tiff-dev tk-dev tcl-dev

RUN mkdir $APP

WORKDIR $APP

COPY requirements.txt /webapps/

RUN pip3 install -r requirements.txt -i https://pypi.douban.com/simple

COPY . /webapps

# COPY ./compose/production/django/start.sh /start.sh
# 转换windows文件格式为unix 
RUN sed -i 's/\r//' /start.sh && chmod +x /start.sh
```

从python:3.6 

> 2) start.sh项目

```shell
#!/bin/sh

python manage.py migrate&&
python manage.py collectstatic --noinput&&
gunicorn blogproject.wsgi.application -w 2 -k gthread -b 0.0.0.0:8000 --chdir=/webapps
```

#### 编排Nginx配置

> 1) Dockerfile配置

```dockerfile
FROM nginx:1.17.1

RUN rm /etc/nginx/conf.d/default.conf

COPY ./componse/production/nginx/aigoo-top.conf /etc/nginx/conf.d/

RUN mkdir -p /usr/share/nginx/html/static

RUN mkdir -p /usr/share/nginx/html/media

EXPOSE 80 8000
```

> 2) aigoo-top.conf配置

```shell

upstream aigoo_top  {
    server aigoo_top:8000;
}

server {
	charset utf-8;  # 服务器字符集
	listen 80; # 监听的端口
	server_name *.aigoo.top aigoo.top www.aigoo.top;
	autoindex on;
	
    location /media {
		alias /usr/share/nginx/html/media;
	}
	location /static {
		alias /usr/share/nginx/html/static;
	}
	location / {
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header Host $http_host;
		proxy_redirect off;
		proxy_pass http://aigoo_top:8000;
	}

	error_page   500 502 503 504  /50x.html;

}
```

> 3) 设置国内镜像源

```shell
deb-src http://archive.ubuntu.com/ubuntu xenial main restricted #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb http://mirrors.aliyun.com/ubuntu/ xenial multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse #Added by software-properties
deb http://archive.canonical.com/ubuntu xenial partner
deb-src http://archive.canonical.com/ubuntu xenial partner
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-security multiverse

deb http://mirrors.tencentyun.com/debian/ jessie main non-free contrib
deb http://mirrors.tencentyun.com/debian/ jessie-updates main non-free contrib
deb http://mirrors.tencentyun.com/debian/ jessie-backports main non-free contrib
deb-src http://mirrors.tencentyun.com/debian/ jessie main non-free contrib
deb-src http://mirrors.tencentyun.com/debian/ jessie-updates main non-free contrib
deb-src http://mirrors.tencentyun.com/debian/ jessie-backports main non-free contrib
deb http://mirrors.tencentyun.com/debian-security/ jessie/updates main non-free contrib
deb-src http://mirrors.tencentyun.com/debian-security/ jessie/updates main non-free contrib

deb http://deb.debian.org/debian stretch main
deb http://security.debian.org/debian-security stretch/updates main
deb http://deb.debian.org/debian stretch-updates main
```



#### docker-compose.yml配置

> docker-compose.yml配置

```yaml
version: '3'

volumes:
  static:
  database:
  esdata:

services:
  hellodjango_blog_tutorial:
    build:
      context: .
      dockerfile: compose/production/django/Dockerfile
    image: hellodjango_blog_tutorial
    container_name: hellodjango_blog_tutorial
    working_dir: /app
    volumes:
      - database:/app/database
      - static:/app/static
    env_file:
      - .envs/.production
    ports:
      - "8000:8000"
    command: /start.sh

  nginx:
    build:
      context: .
      dockerfile: compose/production/nginx/Dockerfile
    image: hellodjango_blog_tutorial_nginx
    container_name: hellodjango_blog_tutorial_nginx
    volumes:
      - static:/apps/hellodjango_blog_tutorial/static
    ports:
      - "80:80"
      - "443:443"

  elasticsearch:
    build:
      context: .
      dockerfile: ./compose/production/elasticsearch/Dockerfile
    image: hellodjango_blog_tutorial_elasticsearch
    container_name: hellodjango_blog_tutorial_elasticsearch
    volumes:
      - esdata:/usr/share/elasticsearch/data
    ports:
      - "9200:9200"
    environment:
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
      nproc: 65536
      nofile:
        soft: 65536
        hard: 65536
```

### 碰到的坑

> 1、django安装python:3.6-alpine包,耗时太久，不要选择python:3.6-alpine包

<font color='red'>问题:</font> python:3.6-alpine包，但是没有gcc、python-dev，少了很多需要的依赖包，特别在pip安装python包，安装一个崩溃一个，都是没有依赖，而安装apk 速度慢得要死，折腾了一天，很疯狂

<font color='green'>解决：</font> 换了一个依赖包，对应Dockerfile如下

```dockerfile
FROM centos/python-36-centos7

ENV PYTHONUNBUFFERED 1

ENV APP /webapps

#RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

# Pillow dependencies
#RUN apk update \&&apk add --no-cache jpeg-dev zlib-dev freetype-dev lcms2-dev openjpeg-dev tiff-dev tk-dev tcl-dev gcc libffi-dev python3-dev openssl-dev bash wget curl git make vim docker  ca-certificates

RUN pip3 install --upgrade pip -i https://pypi.douban.com/simple&& \
pip3 install setuptools_scm -i https://pypi.douban.com/simple

USER root

RUN mkdir $APP

WORKDIR $APP

COPY requirements.txt /$APP/

RUN pip3 install -r requirements.txt -i https://pypi.douban.com/simple

COPY . /$APP

COPY ./compose/production/django/start.sh /start.sh
# 转换windows文件格式为unix 
RUN sed -i 's/\r//' /start.sh && chmod +x /start.sh

EXPOSE 80
```



> 2、默认的数据库编码格式不是utf-8，需要设置mysql数据库格式为utf-8

<font color='red'>问题:</font> 脚本执行 python manage.py migrate ，说users下面的sign字段default值错误，查看下因为default用中文写的，而数据库不支持中文，所以产生问题

<font color='green'>解决：</font> 

1、在本地项目根目录下，创建mysql.cnf,写入

```shell
[client]
default-character-set=utf8
 
[mysqld]
character_set_server=utf8

```

备注： 这里第二个一定是mysqld，而且值是character_set_server=utf8， 网上好多都是mysql， 填写那个配置，mysql并不会更新，仍然出错，我的mysql是5.6版本

2、在宿主机执行 docker cp  mysql.cnf [container_id]:/etc/mysql/conf.d/mysql.cnf

3、重新启动容器，并commit到阿里云备用

```shell
$ sudo docker login --username=xxx@aliyun.com registry.cn-hangzhou.aliyuncs.com
$ sudo docker login --username=xxx@aliyun.com registry.cn-hangzhou.aliyuncs.com
$ sudo docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/my_test_docker_space/mysql_utf8:v1
$ sudo docker push registry.cn-hangzhou.aliyuncs.com/my_test_docker_space/mysql_utf8:v1
```



> 3、安装Collecting django-haystack==2.8.1 出现 ERROR: Command errored out with exit status 1:

<font color='red'>问题:</font> 

```shell
 Downloading https://pypi.doubanio.com/packages/69/43/3e247b7b2134b48e9a53fb387e191e5e05b5f38f2faf78ca892097c2b441/django-haystack-2.8.1.tar.gz (1.6 MB)
    ERROR: Command errored out with exit status 1:
     command: /opt/app-root/bin/python3 -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-t9afht34/django-haystack/setup.py'"'"'; __file__='"'"'/tmp/pip-install-t9afht34/django-haystack/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' egg_info --egg-base /tmp/pip-pip-egg-info-_9xh3654
         cwd: /tmp/pip-install-t9afht34/django-haystack/
    Complete output (33 lines):
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/tmp/pip-install-t9afht34/django-haystack/setup.py", line 71, in <module>
        setup_requires=['setuptools_scm'],
      File "/opt/rh/rh-python36/root/usr/lib64/python3.6/distutils/core.py", line 108, in setup
        _setup_distribution = dist = klass(attrs)
      File "/opt/app-root/lib/python3.6/site-packages/setuptools/dist.py", line 315, in __init__
        self.fetch_build_eggs(attrs['setup_requires'])
```

<font color='green'>解决：</font> 安装pip3 install setuptools_scm，对应的Dockerfile如下

```dockerfile
FROM centos/python-36-centos7

ENV PYTHONUNBUFFERED 1

ENV APP /webapps

#RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

# Pillow dependencies
#RUN apk update \&&apk add --no-cache jpeg-dev zlib-dev freetype-dev lcms2-dev openjpeg-dev tiff-dev tk-dev tcl-dev gcc libffi-dev python3-dev openssl-dev bash wget curl git make vim docker  ca-certificates

RUN pip3 install --upgrade pip -i https://pypi.douban.com/simple&& \
pip3 install setuptools_scm -i https://pypi.douban.com/simple

USER root

RUN mkdir $APP

WORKDIR $APP

COPY requirements.txt /$APP/

RUN pip3 install -r requirements.txt -i https://pypi.douban.com/simple

COPY . /$APP

COPY ./compose/production/django/start.sh /start.sh
# 转换windows文件格式为unix 
RUN sed -i 's/\r//' /start.sh && chmod +x /start.sh

EXPOSE 80
```



> 4、gunicorn在脚本里面启动wsgi的指令是冒号

<font color='red'>问题:</font>  gunicorn blogproject.wsgi.application -w 2 -k gthread -b 0.0.0.0:8000 --chdir=/webapps  在start.sh里面,blogproject和wsgi之间应该用:而不是. ,不过这个配置在pycharm里面没问题

<font color='green'>解决：</font> unicorn blogproject:wsgi.application -w 2 -k gthread -b 0.0.0.0:8000 --chdir=/webapps



> 5、nginx里面无法启动，说aigoo_top可能不存在

<font color='red'>问题:</font>  在nginx里面配置upstream，upstream的服务名字和web服务名字相同，都是aigoo_top，导致出错，错误配置如下

```shell
upstream aigoo_top  {
    server aigoo_top:8000;
}

server {
	charset utf-8;  # 服务器字符集
	listen 80; # 监听的端口
	server_name *.aigoo.top aigoo.top www.aigoo.top;
	autoindex on;

	error_log /tmp/nginx_error.log;
	access_log /tmp/nginx_access.log;

	location /media {
		alias /usr/share/nginx/html/media;
	}
	location /static {
		alias /usr/share/nginx/html/static;
	}

	location / {
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header Host $http_host;
		proxy_redirect off;
		proxy_pass http://aigoo_top:8000;
	}
	
	error_page   500 502 503 504  /50x.html;

}
```



<font color='green'>解决：</font> 把upstream负载均衡服务改名字，把location / 里面的proxy_pass里面也修改下，正确配置如下：

```shell

upstream aigoo_top_upstream  {
    server aigoo_top:8000;
}

server {
	charset utf-8;  # 服务器字符集
	listen 80; # 监听的端口
	server_name *.aigoo.top aigoo.top www.aigoo.top;
	autoindex on;

	error_log /tmp/nginx_error.log;
	access_log /tmp/nginx_access.log;

	location /media {
		alias /usr/share/nginx/html/media;
	}
	location /static {
		alias /usr/share/nginx/html/static;
	}

	location / {
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header Host $http_host;
		proxy_redirect off;
		proxy_pass http://aigoo_top_upstream;
	}
	
	error_page   500 502 503 504  /50x.html;

}
```



> 6、配置完成后，页面访问localhost 无法进入到index页面

<font color='red'>问题:</font>  配置完成后，所有的容器也都启动起来了，这个时候，在浏览器输入localhost，浏览器默认进入到了http://localhost/tutorial/， 并没有进入到首页。

<font color='green'>解决：</font> 现在还不知道为什么，我直接用ip访问没问题，如果用localhost访问就无法进入首页，用127.0.0.1就可以正确进入到首页





> 7、因为一个问题，导致docker-compose up 创建到一半，此时中断了，我解决了这个问题之后，重新执行docker-compose up ，系统告诉我 user_banner表已经存在

<font color='red'>问题:</font>  因为一个问题，导致docker-compose up 创建到一半，此时中断了，我解决了这个问题之后，重新执行docker-compose up ，系统告诉我 user_banner表已经存在。

<font color='green'>解决：</font> 

原因 : 是因为设置了volume /databases， 执行一半的时候中断，但是表单已经创建了，我进入数据库，确实数据库也已经有了blogproject， 我再次build，这个时候系统直接使用原来的databases备份，所以执行Python manage.py migrate， 这个时候告诉我表已经存在

处理: 删除掉所有的卷、网络，然后重新执行docker-compose up

## 实践四、数据库在宿主机，服务器和nginx在docker

### 项目背景介绍

