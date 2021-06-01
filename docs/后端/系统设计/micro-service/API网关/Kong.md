# Kong

教程

https://blog.csdn.net/cry1049208942/article/details/105971198

https://www.jianshu.com/p/54d49faecce3

https://www.jianshu.com/p/9ff063b1f4c6

## 运行 kong

### 创建子网

```bash
docker network create kong-net
```

### 运行数据库

#### 创建postgersql数据库

```bash
docker run -d --name kong-database \
               --network=kong-net \
               -p 5432:5432 \
               -e "POSTGRES_USER=kong" \
               -e "POSTGRES_DB=kong" \
               -e "POSTGRES_PASSWORD=kong" \
               postgres:9.6
```

#### 数据库迁移

```bash
docker run --rm \
     --network=kong-net \
     -e "KONG_DATABASE=postgres" \
     -e "KONG_PG_HOST=kong-database" \
     -e "KONG_PG_PASSWORD=kong" \
     -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
     kong:1.5 kong migrations bootstrap
```

### 运行 kong

```bash
docker run -d --name kong \
     --network=kong-net \
     -e "KONG_DATABASE=postgres" \
     -e "KONG_PG_HOST=kong-database" \
     -e "KONG_PG_PASSWORD=kong" \
     -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
     -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
     -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
     -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
     -p 8000:8000 \
     -p 8443:8443 \
     -p 8001:8001 \
     -p 8444:8444 \
     kong:1.5
```

### 运行 kong gui

#### 运行 kong-dashboard~

> 建议直接跳过，dashboard 可以运行，但是前端尽是报错，github上面的issue也没人处理，应该是不维护了。

```bash
docker run  --network=kong-net \
            --name kong-dashboard \
            -p 8080:8080 \
            -d pgbi/kong-dashboard \
            start --kong-url http://kong:8001 \
            --basic-auth user1=password1 
```

#### 运行 konga（kong可视化管理工具）

```bash
docker pull pantsel/konga

docker run -d \
-p 1337:1337 \
--network kong-net \
-e "TOKEN_SECRET=kongtoken" \
-e "DB_ADAPTER=postgres" \
-e "DB_HOST=kong-database" \
-e "DB_USER=kong"  \
-e "DB_PASSWORD=kong" \
 --name konga \
pantsel/konga
```

访问：http://192.168.28.118:1337/





# 从0开始搭建Prometheus+Grafana

api 经过 kong，会留下日志，prometheus 通过 `8001` 端点标准化采集的日志，
 `grafana` 通过 `prometheus `得到标准后的日志进行展示。



安装教程：https://www.yuque.com/markus/hon7gx/nedufb

使用教程！：https://www.cnblogs.com/guoxiangyue/p/11772717.html

## prometheus

>中文普罗米修斯，全链路实时监控

下载：https://prometheus.io/download/

在tsingdata03的/opt/software

```
wget https://github.com/prometheus/prometheus/releases/download/v2.27.1/prometheus-2.27.1.linux-amd64.tar.gz
```



```
tar -zxvf prometheus-2.27.1.linux-amd64.tar.gz -C /opt/module/
```

启动

```
./prometheus --config.file=prometheus.yml &
```

访问：http://192.168.28.118:9090/



prometheus.yml 文件配置如下

```
cd /opt/module/prometheus
vim prometheus.yml
```



```yaml
# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
    - targets: ['192.168.28.116:8001']

```

核心点就是上面的` targets`，相信很多分析类的文章都没有提到。

重新启动

```
./prometheus --config.file=prometheus.yml &
```



## grafana



```
wget https://dl.grafana.com/oss/release/grafana-7.5.7-1.x86_64.rpm
sudo yum install grafana-7.5.7-1.x86_64.rpm
```

启动

```
sudo service grafana-server start 
```

使用下面命令检查是否启动成功

```
sudo service grafana-server status
```

访问：http://192.168.28.118:3000/

```
admin
admin
```



设置prometheus连接grafana

![image-20210601155739042](images/image-20210601155739042-1622534260171.png)



新建仪表盘

![image-20210601155954715](images/image-20210601155954715.png)







## node_exporter

```
https://github.com/prometheus/node_exporter/releases/download/v1.1.2/node_exporter-1.1.2.linux-amd64.tar.gz
```



解压安装

```
tar -zxvf node_exporter-1.0.1.linux-amd64.tar.gz -C /opt/module/
```



改名

```
mv node_exporter-1.0.1.linux-amd64 node_exporter

cd node_exporter
```

启动服务

```
./node_exporter & 
```

如果一切顺利你会看到类似下面的输出，默认端口是`9100`

访问：http://192.168.28.118:9100/

![Screenshot 2021-01-16 131531.png](https://cdn.nlark.com/yuque/0/2021/png/2878931/1610774162736-70e52bf0-271d-4386-95d6-d369a4f495ae.png?x-oss-process=image%2Fresize%2Cw_1666)

将 node_exporter 服务加入到 Prometheus 的监控列表

```
cd /opt/module/prometheus
```

在 prometheus.yml 末尾添加如下内容，将IP地址换成你的 node_exporter 节点的实际地址即可。

```yml
- job_name: 'node'

   static_configs:

   - targets: ['192.168.28.118:9100']
```

```
./prometheus --config.file=prometheus.yml &
```

访问：http://192.168.28.118:9090/targets

![image-20210601154733089](images/image-20210601154733089.png)





![Screenshot 2021-01-16 132212.png](https://cdn.nlark.com/yuque/0/2021/png/2878931/1610774582657-fbfaadc3-4e4f-4080-a45b-4a7c7ea38e52.png?x-oss-process=image%2Fresize%2Cw_1666)





## dashboard

https://grafana.com/grafana/dashboards 

### 第一个模板

翻到下面

![image-20210601162351448](images/image-20210601162351448.png)

![image-20210601162434371](images/image-20210601162434371.png)

我这里就是下面这个数字

```
13105
```

或者下载JSON，可以不用

![image-20210601162940835](images/image-20210601162940835.png)

 然后打开我们的Grafana监控页面，打开dashboard的管理页面

![img](https://img2018.cnblogs.com/blog/1323857/201911/1323857-20191101105200532-1674361444.png)

 点击【import】按钮

![img](https://img2018.cnblogs.com/blog/1323857/201911/1323857-20191101105300894-376923003.png)

 然后将我们刚才的复制的dashboard Id 复制进去

![img](https://img2018.cnblogs.com/blog/1323857/201911/1323857-20191101105435362-2091882290.png)

 Grafana会自动识别dashboard Id 。

然后点击【change】按钮，生成一个随机的UID，然后点击下方输入框，选择我们之前创建的数据源Prometheus，最后点击【Import】按钮，即可完成导入。

![img](https://img2018.cnblogs.com/blog/1323857/201911/1323857-20191101105819706-383263896.png)

 导入成功后，会自动打开该Dashboard，即可看到我们刚才设置好的node监控

![img](https://img2018.cnblogs.com/blog/1323857/201911/1323857-20191101110044416-439534465.png)

### 第二个模板

https://grafana.com/grafana/dashboards/8919

此模板需要配置，我们这边监控多台服务器，其他机器也需要操作node_exporter的步骤，需要让他们跑起来。

然后再配置

```yml
# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
    - targets: ['192.168.28.118:8001']

  - job_name: 'node'
    static_configs:
    - targets: ['192.168.28.118:9100','192.168.28.117:9100','192.168.28.116:9100']
```



![image-20210601164031144](images/image-20210601164031144.png)

