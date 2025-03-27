---
title: vue和django的一些配置
categories: 配置
---

vue 和 django 的一些配置

<!-- more -->

# 环境配置

## Django 安装

python 3.11 之后需要使用虚拟环境安装，否则就会报错。具体的做法是这样的

首先使用这个命令创建一个 python 虚拟环境 `venv`是虚拟环境名字 后面的是路径可以自己找一个目录放进去

```shell
python -m venv /path/to/new/virtual/environment
```

接着进去这个刚刚创建好的目录去 source 它
比如我的电脑目前的虚拟环境地址在

```shell
~/.venv/
```

执行这个命令

```shell
source ~/.venv/bin/activate
```

这样这个终端的环境就变成虚拟环境 python 了

如果不使用虚拟环境 pip 安装`Django`会直接报错，其他的一些包也是同理

```shell
pip install django
```

运行服务器

```shell
python manager.py runserver
```

## Django 连接 MySQL

MySQL 数据库可以使用命令行直接安装

```shell
sudo apt install mysql-server
```

Django 与 mysql 通讯需要使用 pymysql 这个库（比较老了），新的库配置环境有些问题所以比较推荐使用这个

```shell
sudo pip install pymysql
```

下载好模块后需要在项目的`__init__.py`中配置。注意是项目的`__init__`不是 app 下的。

```python
import pymysql
pymysql.install_as_MySQLdb()
```

接着修改 settings.py 中的 DATABASES 条目

这是 Django 默认的 使用自带的 sqlite3 数据库

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

我们修改一下，以使用 MySQL

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': '',
        'USER':'root',
        'PASSWORD':'123456',
        'HOST':'localhost',
        'PORT':'3306',
    }
}
```

### 服务器 MySQL 连接 navicat

首先登录数据库

```shell
mysql -uroot -p    ## 以root登录数据库
```

查看一下当前端口

```shell
use mysql;    ## 选择mysql数据库
select user,host from user;    ## 查看用户访问端口
```

**说明**：root 用户默认的是 localhost，说明只允许从本地登录 mysql 服务。而我们要从远程以 root 用户连接数据库，就必须修改 host 的值，改为**'%'**：允许任何 ip 访问。

```shell
update user set host = '%' where user = 'root';
```

再次查看

```shell
mysql> select user,host from user;    ## 查看用户访问端口
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| root             | %         |
| debian-sys-maint | localhost |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
+------------------+-----------+
5 rows in set (0.00 sec)

```

这样就表示允许任何 ip 访问了

```shell
FLUSH PRIVILEGES;    ## 刷新服务配置项
```

刷新一下

接着设置一个远程用户的登录密码

```shell
 ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root_pwd'; ## 授权root远程登录 后面的root_pwd代表登录密码
```

这里的 root_pwd 是自己填的 我写的是 123456

## Django 与 Vue 的通讯

### 配置 cnpm

```shell
sudo npm config set registry https://registry.npm.taobao.org
```

```shell
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
```

以后安装 npm 的包都可以使用去安装

```shell
cnpm install [name]
```

使用 vite 创建项目也会有这个问题可以考虑下面的命令进行换源

查看 npm 的镜像地址

```shell
npm config get registry
```

使用 taobao 镜像地址

```shell
npm config set registry https://registry.npm.taobao.org
```

# 项目运行

## Django 篇

### 创建项目

```shell
django-admin startproject xxxx
```

### 创建 APP

进入项目后

```shell
python manage.py startapp xxxx
```

### 项目目录

```shell
├── app01
│   ├── admin.py                          [不动]自带的默认的后台管理功能
│   ├── apps.py                           [不动]app启动类
│   ├── __init__.py
│   ├── migrations                        [不动]数据库变更记录
│   │   └── __init__.py
│   ├── models.py                         [重要]对数据库进行操作
│   ├── tests.py
│   └── views.py                          [重要,经常修改]函数
├── manage.py
└── mysite
    ├── asgi.py
    ├── __init__.py
    ├── __pycache__
    │   ├── __init__.cpython-38.pyc
    │   └── settings.cpython-38.pyc
    ├── settings.py
    ├── urls.py                           [URL->函数]
    └── wsgi.py
```

### APP 注册

创建好的 APP 不能直接使用需要注册后才可以，小型项目的话一般只需要一个 APP 就可以了。注册需要在项目名的`seetings.py`中注册

在这个`mysite`的项目中就是`mysite.settings.py`

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app01.apps.App01Config', #这里也可以直接简写为app01
]
```

### 编写 URL 和视图函数的对应关系

在 urls.py 中编写 路径是`mysite.urls.py`

```python
from django.contrib import admin
from django.urls import path


from app01 import views
urlpatterns = [
    # path('admin/', admin.site.urls),

    path('index/',views.index)

]
```

下面即是 app01.views.py 的内容

```python
from django.shortcuts import render,HttpResponse

# Create your views here.

def index(request): #默认有一个参数 request
    return HttpResponse("Hello")
```

### 模板路径问题

下面是使用 `django-admin` start project 创建的项目可以看到 DIRS 里面是空的，如果我们有一个 template 这里是放在 APP01 的文件夹里面

```shell

├── app01
│   ├── admin.py
│   ├── apps.py
│   ├── __init__.py
│   ├── migrations
│   │   ├── __init__.py
│   │   └── __pycache__
│   │       └── __init__.cpython-38.pyc
│   ├── models.py
│   ├── __pycache__
│   │   ├── admin.cpython-38.pyc
│   │   ├── apps.cpython-38.pyc
│   │   ├── __init__.cpython-38.pyc
│   │   ├── models.cpython-38.pyc
│   │   └── views.cpython-38.pyc
│   ├── templates
│   │   └── user_list.html
│   ├── tests.py
│   └── views.py
├── db.sqlite3
```

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

```

Django 会根据注册的 APP 顺序依次查找 template 文件夹

```python

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app01.apps.App01Config',
]
```

但是如果使用 pycharm 就会在 TEMPLATES 这里面加上一句

```python
 'DIRS': [], # 命令行创建 建议使用这种 这样每个模板都被APP归类
 'DIRS': [os.path.join(BASE_DIR,'templates')], # 使用pycharm会变成这样
```

pycharm 这样的结果就是 django 会优先在项目根目录去寻找 template

### 静态文件(css,js,图片)的引入

```html
{% load static %}
<!DOCTYPE html>
<head>
	<meta charset="utf-8">
	<title>标题</title>
</head>

<body>
	<h1>这是用户列表</h1>
	<img src="{% static 'img/ming_ri_xiang.png' %}">
</body>

</html>
```

### Django 模板语法

响应式的那种，就是在 HTML 中留一些位置，之后用数据对这些位置进行处理

```html
{% load static %}
<!DOCTYPE html>

<head>
	<meta charset="utf-8">
	<title>标题</title>
	<link rel="stylesheet" href="{% static 'plugins/bootstrap/css/bootstrap.css'%}">
</head>

<body>
	<h1>这是用户列表</h1>
	<!-- <img src="{% static 'img/ming_ri_xiang.png' %}"> -->
	<input type="text" class="btn btn-primary" value="新建">
	<div>{{ n1 }}</div>
	<div>{{n2.0}}</div>
	<div>{{n2.1}}{{n2.2}}</div>
	<div>
		{% for item in n2 %}
		<span>{{ item }}</span>
		{% endfor %}
	</div>
</body>
<hr />
{{n3.salary}}

<ul>
	{% for item in n3.keys %}
	<li>{{ item }}</li>
	{% endfor %}
</ul>

</html>
```

后台 views

```python
from django.shortcuts import render,HttpResponse

# Create your views here.

def index(request): #默认有一个参数 request
    name = "长门有希大萌神" #这里可以通过python从数据库里面读数据
    roles = ['A','B','C']
    user_info = {"name":"qw","salary":10000,"role":"CTO"}
    return render(request,"index.html",{"n1":name,"n2":roles,"n3":user_info})

def user_list(request):
    return render(request,"user_list.html")

def user_add(request):
    return HttpResponse("添加用户")
```

### 请求和响应

```python
def something(request):
    #请求的三种方式
    print(request.method) #获得请求方式
    print("URL=> ",request.GET) #在URL上传值
    print("POST=> ",request.POST)#在请求体中返回数据

    #响应
    # return HttpResponse("添加用户")
    # return render(request,"user_list.html")

    return redirect("https://www.baidu.com") # 让浏览器重定向到其他页面
```

### Django 与数据库交互

连接好数据库后，打开 app01 下的 models.py

```python
from django.db import models

# Create your models here.
class UserInfo(models.Model):
    user = models.CharField(max_length=32)
    pwd = models.CharField(max_length=32)
```

这样就会在我们的数据库中创建一个名为 UserInfo 的表，且有两个字段 user 和 pwd，当然这个时候只是设计好，下面几个命令执行完后才会真正开始创建

运行下面这个命令来创建数据库表

```shell
python manage.py makemigrations
```

再输入命令

```shell
python manage.py migrate
```

# 前后端交互

## Django

### Django setting 文件配置

django 想要与 Vue 进行交互需要 RestFramework，还有 cors-headers 这两个插件

```shell
pip install djangorestframework
pip install django-cors-headers
```

安装在 django 的根目录下的 setting 文件中进行 app 注册

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'corsheaders',
    'lyb',
]
```

因为前端与后端进行交互设计一个跨域保护的问题，后端使用 django-cors-headers 插件

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'corsheaders.middleware.CorsMiddleware', # 注意顺序
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

接着继续编辑 setting 文件，增加这两个配置

```python
ROOT_URLCONF = 'backends.urls' # 后端项目的总路由路径
CORS_ORIGIN_ALLOW_ALL = True #这是允许所有的跨域请求
```

至此后端的 setting 文件配置完成

总配置文件如下 backends.settings.py

```python
"""
Django settings for backends project.

Generated by 'django-admin startproject' using Django 5.0.1.

For more information on this file, see
https://docs.djangoproject.com/en/5.0/topics/settings/

For the full list of settings and their values, see
https://docs.djangoproject.com/en/5.0/ref/settings/
"""

from pathlib import Path

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent


# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/5.0/howto/deployment/checklist/

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'django-insecure-#fz@#++_qt+!=&)tt2&%uui8d)pmh=ko0v18midrnv8vfud@^*'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = []


# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'corsheaders',
    'lyb',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

ROOT_URLCONF = 'backends.urls'
CORS_ORIGIN_ALLOW_ALL = True
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'backends.wsgi.application'


# Database
# https://docs.djangoproject.com/en/5.0/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}


# Password validation
# https://docs.djangoproject.com/en/5.0/ref/settings/#auth-password-validators

AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]


# Internationalization
# https://docs.djangoproject.com/en/5.0/topics/i18n/

LANGUAGE_CODE = 'en-us'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_TZ = True


# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/5.0/howto/static-files/

STATIC_URL = 'static/'

# Default primary key field type
# https://docs.djangoproject.com/en/5.0/ref/settings/#default-auto-field

DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'

```

### Django 的总路由配置

```python
from django.contrib import admin
from django.urls import path,include
from rest_framework import routers    #这些都要引用
from lyb.views import LybViewSet      #这些都要引用

router = routers.DefaultRouter()
router.register(r'lyb',LybViewSet)    #这个lyb是你的app

urlpatterns = [
    path('admin/', admin.site.urls),   #这句可以删掉
    path('api/',include(router.urls))  #这句是与自己的app的配置有关
]
```

### Django 配置序列化

app 名字.serializers.py 没有就创建

```python
from rest_framework import serializers
from .models import Lyb

class LybSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Lyb
        fields = "__all__" #所有的字段都序列化
```

models.py 中的内容 ，_虽然不是前后端交互必要的但是因为是一个案例于是留下来_

```python
from django.db import models

# Create your models here.
class Lyb(models.Model):
    title = models.CharField(max_length=100)
    author = models.CharField(max_length=50)
    content = models.TextField()
    posttime = models.DateTimeField(auto_now_add=True)
```

下面的是 views 的内容

```python
from django.shortcuts import render
from rest_framework import viewsets
from .models import Lyb #导入自己的appmodel
from .serializers import LybSerializer # 导入序列化器
class LybViewSet(viewsets.ModelViewSet):
    queryset = Lyb.objects.all().order_by('-posttime') #按时间排序 当然也可以选择其他的排序
    serializer_class =  LybSerializer
```

### 获得 model 中的数据 特定条件

```python
from myapp.models import MyModel

specific_object = MyModel.objects.get(field=value)
```

### 获得 model 中的全部数据

```python
from myapp.models import MyModel

all_data = MyModel.objects.all()
```

### 视图中获得对象的值

```python
import json
from django.http import JsonResponse

def login_view(request):
    if request.method == 'POST':
        data = json.loads(request.body)
        username = data.get('username')
        password = data.get('password')
        # 进行登录逻辑处理
        # ...
        # 返回响应数据
        response_data = {
            'message': '登录成功',
            'username': username,
        }
        return JsonResponse(response_data)
```

### 常见写法

```python
from django.http import JsonResponse
from .models import UserModel

def login_view(request):
    if request.method == "POST":
        # 获取请求中的用户名和密码
        username = request.POST.get("username")
        password = request.POST.get("password")

        # 在这里进行用户登录验证逻辑
        try:
            user = UserModel.objects.get(userName=username, passWord=password)
            # 用户登录成功
            return JsonResponse({"message": "登录成功"})
        except UserModel.DoesNotExist:
            # 用户名或密码不正确，登录失败
            return JsonResponse({"message": "用户名或密码不正确"}, status=401)

    # 如果不是 POST 请求，返回错误响应
    return JsonResponse({"message": "无效的请求"}, status=400)
```

## VUE

### VUE 环境配置

下载 axios

```shell
npm install axios -s
```

使用 vite 脚手架创建项目 不适用 vue-cli

```shell
npm init vite-app <project-name>
```

vue 中默认的代码很多都是没用的因此我们删除

vue 项目的 index.html 如下

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>vue-django</title>
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD"
      crossorigin="anonymous"
    />
  </head>
  <body>
    hello
    <div class="container">
      <div id="app"></div>
    </div>
    <script type="module" src="/src/main.js"></script>
    <!-- 注意这一行 type和src的路径都要打 -->
  </body>
</html>
```

把 main.js 中无用的代码删除

```js
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```

创建一个组件（组件名随意我这里的例子交 lyb）这边是前端向后端要数据的核心部分

```vue
<template>
  <div class="row">
    <div class="col-md-8">
      <table class="table">
        <thead>
          <tr>
            <th>标题</th>
            <th>作者</th>
            <th>内容</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in ly_list" :key="item.url">
            //循环显示元素
            <td>{{ item.title }}</td>
            <td>{{ item.author }}</td>
            <td>{{ item.content }}</td>
            <td>测试</td>
            <td>
              <button class="btn btn-success" title="编辑">
                <i></i>
              </button>
              <button class="btn btn-danger" title="删除">
                <i></i>
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
  import axios from 'axios' //导入axios库
  import { reactive, onMounted, toRefs } from 'vue' //这三个都要导入
  export default {
    name: 'lyb',
    setup() {
      let base_url = 'http://127.0.0.1:8000/api/lyb/' //这是django服务器的地址就是rest_framework提供数据的网址，
      const state = reactive({
        ly_list: [] //用来接数据
      })
      const getLyb = () => {
        axios
          .get(base_url)
          .then((res) => {
            //发送请求
            console.log('成功')
            state.ly_list = res.data
            console.log(res.data)
          })
          .catch((err) => {
            console.log(err)
          })
      }
      onMounted(() => {
        getLyb() //需要onMounted挂载
      })

      return {
        ...toRefs(state) //这段也要加上
      }
    }
  }
</script>
```

最后在 APP.vue 中注册一下

```vue
<template>
  <div class="row">
    <lyb />
  </div>
</template>

<script>
  import lyb from './components/Lyb.vue'
  export default {
    name: 'App',
    components: {
      lyb
    }
  }
</script>
```

将 django 与 vue 的服务器都启动，正常情况下就可以看到后端传来的数据了。

这里有个小 bug 使用 vscode ssh 连接的时候网站可能不会发生改变，这并不是代码的问题。可能与电脑/虚拟机性能有关。可以进入虚拟机桌面的终端直接运行，不通过 ssh 转发查看效果

### 使用 element-plus

```shell
npm install element-plus --save
```

**main.js 中配置\*\***

```js
import { createApp } from 'vue'
import App from './App.vue'
import './index.css'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'

const app = createApp(App)
app.use(ElementPlus).mount('#app')
```
