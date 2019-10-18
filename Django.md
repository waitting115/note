# Django

先下载安装python，在官网找资源在迅雷下载

然后打开cmd，用pip安装django

​	命令：pip install django

​	注意，这里可能安装失败，解决方法：更新pip，如果也失败，就多更新几次，就好了，安装django的时候也一样，失败就多更新几次，而且，更新的时候不要弄别的，静静的等着

django安装完成后，路径：C:\Users\Wei\AppData\Local\Programs\Python\Python38\Scripts

然后 新建一个文件夹，打开，从里面打开cmd，输入django-admin startproject mysite，初始化一个项目，

打开新建的项目python manage.py startapp app01，初始化一个app

然后在cmd输入命令：python manage.py runserver 8080，启动你的项目

然后在浏览器输入127.0.0.1:8080，打开你的项目