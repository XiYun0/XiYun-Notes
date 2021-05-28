# 安装

http://nginx.org/en/download.html

alibaba cloud centos安装nginx教程：https://www.cnblogs.com/wuxu-dl/p/10516325.html



```
tar -zxvf nginx-1.20.0.tar.gz -C /opt/module/
```



pcre-8.35

```
wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz

tar -zxvf pcre-8.35.tar.gz -C /opt/module/

cd /opt/module/pcre-8.35/

./configure

make && make install

# 版本
pcre-config --version
```



配置安装

```
cd nginx-1.20.0/

./configure --prefix=/opt/module/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/opt/module/pcre-8.35

make && make install
```



## 启动

```
sbin/nginx
```





```
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       80;
        server_name  localhost;

		location / {
            root   /opt/module/mas/max-ui;
			try_files $uri $uri/ /index.html;
            index  index.html index.htm;
        }
		
		location /prod-api/{
			proxy_set_header Host $http_host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header REMOTE-HOST $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_pass http://localhost:8080/;
		}

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```



```
server {
	listen       80;  # 前端的端口
    server_name  localhost; # 不建议用localhost，Linux的ip地址
 
	location / {
	        root   /opt/module/mas/mas-ui; # 前端的包所在路径
			try_files $uri $uri/ /index.html; # 按此顺序查找请求的文件
            index  index.html index.htm;
        }
		
	# 生产环境的请求都是以/prod-api，可以按F12随便找一个请求看看它的路径
	location /prod-api/{
		proxy_set_header Host $http_host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header REMOTE-HOST $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_pass http://localhost:8080/; # 转发到后端
	}
	
	location /boom {
		proxy_redirect off;
		proxy_pass http://localhost:8080/;
		proxy_set_header Host $http_host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	}
 
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```



## 一些命令

```
(base) [root@glong sbin]# ./nginx -s reload   	# 重新加载配置
(base) [root@glong sbin]# ./nginx -s reopen		#  
(base) [root@glong sbin]# ./nginx -s stop    	# 停止 Nginx
```

