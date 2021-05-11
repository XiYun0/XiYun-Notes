# Superset部署

## 前置要求

```
- Python3
- Anaconda/miniconda
```

### 安装Anaconda

https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/

下载上传到tsingdata02的/opt/software

```
bash Miniconda3-py39_4.9.2-Linux-x86_64.sh
```

一路回车、yes

![img](https://cdn.nlark.com/yuque/0/2020/png/519413/1599734808927-b3da7918-4a09-4679-9239-e59c04ff8064.png)

默认路径：/root/miniconda3

我们要修改为  /opt/module/miniconda3

![img](https://cdn.nlark.com/yuque/0/2020/png/519413/1599734891859-907e3bb9-2c43-41a4-9034-de1b6c717f90.png)

安装完毕！



添加为最新miniconda路径

```
vim ~/.bashrc 
```

```
export PATH="/opt/module/anaconda3/bin:$PATH"
```

```
source ~/.bashrc 
```



重新打开，base为默认环境  3.9.1 是默认的python版本

```
(base) [root@tsingdata01 software]# python
Python 3.9.1 (default, Dec 11 2020, 14:32:07) 
[GCC 7.3.0] :: Anaconda, Inc. on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

是否每次登陆自动激活base环境，

```
conda config --set auto_activate_base false
```

再次开启就不会有base了



激活base等环境

```
conda activate 需要激活的环境，如base
```



## 安装Superset

### 环境

使用pip或者conda（Python的包管理工具），和centos的yum类似。

配置conda国内镜像

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
```

显示下载地址：

```
conda config --set show_channel_urls yes
```

创建python3.6环境（不创建而使用默认的也可以）

```
conda create -name superset python=3.6  # 退出base环境，或者重启再执行这个命令
```

```
[root@tsingdata01 ~]# conda activate superset
(superset) [root@tsingdata01 ~]#
```

conda环境管理命令

```markdown
1. 创建环境：conda create -n env_name

2. 查看所有环境：conda info -envs

3. 删除环境: conda remove -n env_name --all
```

激活虚拟环境

```
conda activate 
```

关闭虚拟环境

```
conda deactivate
```



### 部署Superset

官网 [http://superset.apache.org/installation.html](http://superset.apache.org/installation.html#getting-started)

安装python 和 其他依赖

```
yum install -y python-setuptools

yum install -y gcc gcc-c++ libffi-devel python-devel python-pip python-wheel openssl-devel cyrus-sasl-devel openldap-devel
```

>`遇到的问题`：-bash: /usr/bin/yum: /usr/bin/python: 坏的解释器: 没有那个文件或目录
>
>`解决方法`：
>
>修改以下配置文件
>
>```
>vim /usr/bin/yum
>vim /usr/libexec/urlgrabber-ext-down
>```
>
>```
>#!/usr/bin/python2 -> #!/usr/bin/python2.7
>```
>

setuptools pip升级至最新版

```
pip install --upgrade setuptools pip -i https://pypi.douban.com/simple 
```

安装Superset

```
pip install apache-superset -i https://pypi.douban.com/simple 
```

初始化superset数据库

```
superset db upgrade
```

创建管理员用户

```
export FLASK_APP=superset
flask fab create-admin
```

```sh
[root@tsingdata02 ~]# flask fab create-admin
Username [admin]: tsingdata
User first name [admin]: 
User last name [user]: 
Email [admin@fab.org]: 
Password: 123456
Repeat for confirmation: 
logging was configured successfully
INFO:superset.utils.logging_configurator:logging was configured successfully
/opt/module/miniconda3/lib/python3.9/site-packages/flask_caching/__init__.py:201: UserWarning: Flask-Caching: CACHE_TYPE is set to null, caching is effectively disabled.
  warnings.warn(
No PIL installation found
INFO:superset.utils.screenshots:No PIL installation found
Recognized Database Authentications.
Admin User tsingdata created.
```

![image-20210511153005739](images/image-20210511153005739.png)

初始化

```
superset init 
```

安装gunicorn，这是类似tomcat的python web服务器

```
pip install gunicorn -i https://pypi.douban.com/simple
```

**启动**

```
gunicorn --workers 5 --timeout 120 --bind tsingdata02:8787 "superset.app:create_app()" --daemon
```

>- workers：指定进程个数
>- timeout： worker 进程超时时间，超时会自动重启
>- bind：绑定本机地址，即为Superset访问地址
>- daemon：后台运行4
访问：http://192.168.157.129:8787/

```
用户名 tsingdata
密码	123456
```

![image-20210511153551074](images/image-20210511153551074.png)

停止

```
ps -ef | awk '/superset/ && !/awk/{print $2}' | xargs kill -9
```



# Superset使用

## 对接 MySQL 数据源

### 安装依赖

```
# 安装连接 MySQL 数据源的依赖
conda install mysqlclient
```

>注意：对接不同的数据源，需安装不同的依赖，以下地址为官网说明
>
>http://superset.apache.org/installation.html#database-dependencies

```
# 重启 Superset

ps -ef | awk '/superset/ && !/awk/{print $2}' | xargs kill -9

gunicorn --workers 5 --timeout 120 --bind tsingdata02:8787 "superset.app:create_app()" --daemon
```



### 数据源配置

1、Database 配置

- 点击 Sources/Databases

![image-20210511161445395](images/image-20210511161445395.png)

![image-20210511161508261](images/image-20210511161508261.png)

```
# 点击填写 Database 及 SQL Alchemy URI

Database: xxx
SQLAIchemy URI: mysql://root:root@192.168.157.128:3306/xxx?charset=utf8
```

>注：SQL Alchemy URI 编写规范：mysql://账号:密码@IP:post/数据库名称

- 点击 Test Connection，右下角出现下图提示即表明成功

![image-20210511161923486](images/image-20210511161923486.png)





