

建表

```sql
CREATE TABLE 表名称
(
列名称1 数据类型,
列名称2 数据类型,
列名称3 数据类型,
....
)
```



```sql
CREATE TABLE wt_data(
    NC001 DECIMAL COMMENT'风速ws-wind_speed',
    TR002 DECIMAL COMMENT'电力gp-grid_power',
    YW002 DECIMAL COMMENT'偏航角yaw',
    RT101 DECIMAL COMMENT'发电机仪表温度generator_bearing_temperature_a-ba',
    TR015 VARCHAR(255) COMMENT'风力发电机状态-wt_status',
    c INT COMMENT'风力发电机变量',
    s DATETIME COMMENT'时间'
)
```

```sql
CREATE TABLE result_log(
      start_time DATETIME COMMENT'开始时间',
      end_time DATETIME COMMENT'结束时间',
      deviceNum INT COMMENT'设备数量',
      status_code INT COMMENT'状态码',
      troubleType VARCHAR(255) COMMENT'问题类型',
      actualOccurTime VARCHAR(255) COMMENT'当前发生时间',
      dataJson VARCHAR(255) COMMENT'数据JSON'
)
```



''配置文件中

```
   wtid_var: c
   ws: NC001
   gp: TR002
   yaw: YW002
   dt: s
   ba: RT101
   wt_status: TR015
```

插入

```sql
INSERT INTO data.wt_data (NC001, TR002, YW002, RT101, TR015, c, s)
VALUES (16.64172, 1554.6, 111.2447, 33.6, '1', 1, '2018-06-04 00:00:00');
```

