# 前言 #

Demo修改与研究

# 一、准备工作 #


## 1.1配置数据库 ##

创建三个数据库

```sql

create database cloudDB01;
create database cloudDB02;
create database cloudDB03;

```

三个库都创建相同的表


```sql


CREATE TABLE Dept
(
    deptno INT PRIMARY KEY,
    dname VARCHAR(50),
    db_source VARCHAR(50)
);

```

为表插入数据,db_source的值根据1,2,3**分别标识**(cloudDB01库对应的是1,cloudDB02库对应的是2)

```sql
-- database 1

INSERT INTO dept (deptno, dname, db_source) VALUES (1, 'Java1y', '1');
INSERT INTO dept (deptno, dname, db_source) VALUES (2, 'Java2y', '1');
INSERT INTO dept (deptno, dname, db_source) VALUES (3, 'Java3y', '1');
INSERT INTO dept (deptno, dname, db_source) VALUES (4, 'Java4y', '1');
INSERT INTO dept (deptno, dname, db_source) VALUES (5, 'Java5y', '1');

-- database 2


INSERT INTO dept (deptno, dname, db_source) VALUES (1, 'Java1y', '2');
INSERT INTO dept (deptno, dname, db_source) VALUES (2, 'Java2y', '2');
INSERT INTO dept (deptno, dname, db_source) VALUES (3, 'Java3y', '2');
INSERT INTO dept (deptno, dname, db_source) VALUES (4, 'Java4y', '2');
INSERT INTO dept (deptno, dname, db_source) VALUES (5, 'Java5y', '2');

-- database 3


INSERT INTO dept (deptno, dname, db_source) VALUES (1, 'Java1y', '3');
INSERT INTO dept (deptno, dname, db_source) VALUES (2, 'Java2y', '3');
INSERT INTO dept (deptno, dname, db_source) VALUES (3, 'Java3y', '3');
INSERT INTO dept (deptno, dname, db_source) VALUES (4, 'Java4y', '3');
INSERT INTO dept (deptno, dname, db_source) VALUES (5, 'Java5y', '3');
```

# 二、模块之间解释 #


# 2.0通用API #

创建Dept实体，各个模块都可以使用了(不用每个微服务都创建一个Dept对象)。同时Service接口，Feign的代码也在那里编写。

- microservicecloud-api

# 2.1服务注册中心 #


Eureka服务注册中心(集群)

- microservicecloud-eureka-7001
- microservicecloud-eureka-7002
- microservicecloud-eureka-7003


## 2.2服务提供方 ##

Eureka服务提供方(集群)

- microservicecloud-provider-dept-8001
- microservicecloud-provider-dept-8002
- microservicecloud-provider-dept-8003


带有hystrix功能的服务提供方(hystrix Demo)：

- microservicecloud-provider-dept-hystrix-8004

## 2.3服务消费方 ##

使用restTemplate+ribbon的方式来测试(ribbon自定义了负载均衡算法)

- microservicecloud-consumer-dept-80


使用feign来调用远程服务

- microservicecloud-consumer-dept-feign(直接调用microservicecloud-api的service接口)


监控消费方的指标(性能)

- microservicecloud-consumer-hystrix-dashboard


使用方式：

- 启动consumer-hystrix-dashboard项目，打开`http://localhost:9001/hystrix.stream`


![](https://i.imgur.com/yoDFMHg.png)

我们可以监控：`microservicecloud-provider-dept-hystrix-8001`这个项目，于是在输入栏输入`http://localhost:8004/hystrix.stream`

![](https://i.imgur.com/pBhQJkD.png)

随后，我们去测试接口：`http://localhost:8001/dept/get/7`


监控的数据就会变化了：





## 2.4网关 ##


用于转发路由，服务过滤(安全验证)，限流等等：

- microservicecloud-zuul-gateway-9527

## 2.5Cloud配置文件 ##

SpringCloud Config服务端(获取配置都从这里来拿)

- microservicecloud-config-3344


SpringCloud Config 客户端：

- microservicecloud-config-client-3355
- microservicecloud-config-dept-client-8001
- microservicecloud-config-eureka-client-7001




## 2.6tip ##

如果使用的是IDEA的话，使用 Run dashboard比较方便~






