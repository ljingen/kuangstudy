### 配置镜像加速器

![image-20201011213444775](F:\MD格式学习笔记库\image-20201011213444775.png)



{
  "registry-mirrors": [
    "https://utrarz9k.mirror.aliyuncs.com"
  ],
  "insecure-registries": [],
  "debug": true,
  "experimental": false
}



### 配置File Share 

![image-20201011213516559](F:\MD格式学习笔记库\image-20201011213516559.png)

> 这个非常重要，我在执行docker-compose，就是因为没有设置正确的File share，导致