[官方网站](https://www.sonatype.com/nexus-repository-oss)  
[压缩包下载地址](https://help.sonatype.com/repomanager3/download/download-archives---repository-manager-3)  

### 安装配置
名称 | 信息 | 备注
-|-|-
操作系统 | Ubuntu 16.04 server 64bit | 
JAVA_HOME | /usr/local/jdk | 
nexus安装路径 | /opt/nexus-workspace | 下载压缩包解压到此目录下
nexus版本 | 3.24.0-02 |
端口号 | 8080 |

### 修改配置文件
配置文件```/opt/nexus-workspace/nexus/etc```  
```
application-port=8080
```
> 这里就只改了一下端口号，原来是8081，其实完全不用改，主要是看心情  

配置java路径```/opt/nexus-workspace/nexus/bin/nexus```:  
```
INSTALL4J_JAVA_HOME_OVERRIDE=/usr/local/jdk
```
> [官方说明](https://help.sonatype.com/repomanager3/installation/system-requirements#SystemRequirements-Java)  
> 这里需要在文件开始位置增加INSTALL4J_JAVA_HOME_OVERRIDE  

### 配置systemd
增加配置文件```/lib/systemd/system/nexus.service```:  
```
[Unit]
Description=nexus service
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
ExecStart=/opt/nexus-workspace/nexus/bin/nexus start
ExecStop=/opt/nexus-workspace/nexus/bin/nexus stop
Restart=always
TimeoutSec=600

[Install]
WantedBy=multi-user.target
```
> [官方说明](https://help.sonatype.com/repomanager3/installation/run-as-a-service#RunasaService-systemd)  

执行命令:
```
systemctl daemon-reload
systemctl start nexus
```

### 配置nginx
```
server {
    listen   80;
    server_name  nexus.buxingxing.com;

    client_max_body_size 1G;

    location / {
      proxy_pass http://127.0.0.1:8080/;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
> 因为制品库可能会上传较大文件，因此需要设置nginx最大上传大小，  
> 官方文档示例中使用了1G，这个值可以根据实际需求设置  

### 登录
登录时会有提示查看密码:  
```
cat /opt/nexus-workspace/sonatype-work/nexus3/admin.password
```
登录后修改密码  
```
账号: admin
密码: admin
```
同时开启匿名下载  
```
Enable anonymous access
```
