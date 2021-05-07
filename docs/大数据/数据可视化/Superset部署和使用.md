# Superset部署

## 前置要求

```
- Python3.6  3.7有bug
- Anaconda
```



### 安装Anaconda

https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/

下载上传

[root@bigdata01 software]# bash Anaconda3-2020.07-Linux-x86_64.sh

![img](https://cdn.nlark.com/yuque/0/2020/png/519413/1599734808927-b3da7918-4a09-4679-9239-e59c04ff8064.png)

默认路径：/root/anaconda3

我们要修改为/opt/module/anaconda3

![img](https://cdn.nlark.com/yuque/0/2020/png/519413/1599734891859-907e3bb9-2c43-41a4-9034-de1b6c717f90.png)

安装完毕！



重新打开bigdata01  base为默认环境  3.7.4是默认的python版本

![img](https://cdn.nlark.com/yuque/0/2020/png/519413/1599734970738-76663384-a810-48a5-a7a8-1f5f8ab341fb.png)

是否每次登陆自动激活base环境，(base) [root@bigdata01 ~]# conda config --set auto_activate_base false 

再次开启bigdata01

![img](https://cdn.nlark.com/yuque/0/2020/png/519413/1599735144363-939b3c24-47ed-4d4e-87d0-01bb8c064c43.png)

问题：如果卸载之后再安装，找不到目录

vim ~/.bashrc 中查看PATH配置

添加为最新miniconda路径

```
export PATH="/opt/module/anaconda3/bin:$PATH"
```

## 安装Superset

### 环境

使用pip或者conda（Python的包管理工具），和centos的yum类似。

配置conda国内镜像

```
conda config --add channels 

conda config --add channels 

显示下载地址：conda config --set show_channel_urls yes
```



创建python3.6环境（不创建而使用默认的3.7也一样）

```
[root@bigdata01 ~]# conda create -name superset python=3.6  网络要好（翻？）不行就用base环境吧！
```

附件：

```
conda环境管理命令

创建环境：conda create -n env_name  

查看所有环境：conda info -envs

删除环境: conda remove -n env_name --all
```

激活虚拟环境

```
conda activate base
```

关闭虚拟环境

```
conda deactivate
```



### 部署Superset

官网 [http://superset.apache.org/installation.html](http://superset.apache.org/installation.html#getting-started)

- 安装python 和 其他依赖

```
sudo yum install -y python-setuptools

sudo yum install -y gcc gcc-c++ libffi-devel python-devel python-pip python-wheel openssl-devel cyrus-sasl-devel openldap-devel
```

- setuptools pip升级至最新版

```
pip install --upgrade setuptools pip -i https://pypi.douban.com/simple 
```

- 安装Superset

```
pip install superset -i https://pypi.douban.com/simple 
```



### 配置 superset

#### 创建管理员用户

```
fabmanager create-admin --app superset
```

*执行的时候会有这么一行提示：*

```
fabmanager is going to be deprecated in 2.2.X, you can use the same commands on the improved 'flask fab <command>'
```

但是如果换成 `flask fab create-admin --app superset`，会报错：`Error: no such option: --app`

接着会让你输入用户名（username）、Fist Name、Last Name、邮箱地址、密码、重复密码。

如果遇到以下错误：

```
Was unable to import superset Error: cannot import name '_maybe_box_datetimelike'
```

原因是 `pandas` （在安装 `superset` 时会做为依赖被安装上）版本太高，需要卸载，再安装低版本的 `pandas`。

```
pip install pandas==0.23.4
```

然后重新执行创建管理员用户的命令：

```
fabmanager create-admin --app superset
```

- 初始化Superset数据库  

superset db upgrade



如果遇到错误：

```
...
  File "/root/code/python/msuperset/lib/python3.6/site-packages/sqlalchemy/orm/query.py", line 2572, in _join_determine_implicit_left_side
    "Can't determine which FROM clause to join "
sqlalchemy.exc.InvalidRequestError: Can't determine which FROM clause to join from, there are multiple FROMS which can join to this entity. Try adding an explicit ON clause to help resolve the ambiguity.
```

说明 `sqlalchemy` 版本太高了，要降级 `sqlalchemy`：

```
pip install sqlalchemy==1.2.18
```



（不要用下面的！）

```
pip install flask -i https://pypi.douban.com/simple
pip install wtforms_json -i https://pypi.douban.com/simple
pip install flask_appbuilder -i https://pypi.douban.com/simple
pip install flask_compress -i https://pypi.douban.com/simple
pip install celery -i https://pypi.douban.com/simple
pip install flask_migrate -i https://pypi.douban.com/simple
pip install flask_talisman -i https://pypi.douban.com/simple
pip install flask_caching -i https://pypi.douban.com/simple
pip install sqlparse -i https://pypi.douban.com/simple
pip install bleach -i https://pypi.douban.com/simple
pip install markdown -i https://pypi.douban.com/simple
pip install numpy -i https://pypi.douban.com/simple
pip install pandas -i https://pypi.douban.com/simple
pip install parsedatetime -i https://pypi.douban.com/simple
pip install pathlib2 -i https://pypi.douban.com/simple
pip install simplejson -i https://pypi.douban.com/simple
pip install humanize -i https://pypi.douban.com/simple
pip install geohash -i https://pypi.douban.com/simple
```

##### **载入测试数据**

```
superset load_examples
```

##### **初始化角色和权限**

```
superset init
```

#### **启动服务**

端口号 `8088`，使用 -p 更改端口号

```
superset runserver
```

然后在浏览器中打开 [http://localhsot:8088](https://link.zhihu.com/?target=http%3A//localhsot%3A8088/) 就可以看到 superset 了

#### **添加 mysql 数据源时出错**

提示

```
ModuleNotFoundError: No module named 'MySQLdb'
```

因为 MySQLdb 是 python2 的产品。可使用 mysqlclient 方式代替。要安装 `mysqlclient`：

先安装需要用到的系统依赖：

`Ubuntu` 下：

```
sudo apt-get install python3-dev libmysqlclient-dev
```

`CentOS` 下：

```
yum install python-devel mysql-devel
```

然后安装 `mysqlclient`：

```
pip install mysqlclient
```

其它操作参考： [轻量级BI工具Superset的搭建与使用](https://link.zhihu.com/?target=https%3A//www.jianshu.com/p/b02fcea7eb5b)

- 注册管理员admin用户  

```
export FLASK APP=superset
flask fab create-admin
```

> 说明： flask 是python web的框架（类似于Java的springboot），superset使用的就是flask



## Superset使用



### 对接MySQL数据源

conda install mysqlclient

执行上一步前，请重启superset

