[安装文档](https://www.jenkins.io/zh/doc/book/installing/#debianubuntu)  

### 安装配置
名称 | 信息 | 备注
-|-|-
操作系统 | Ubuntu 16.04 server 64bit | 
JAVA_HOME | /usr/local/jdk | 
jenkins版本 | 2.235.1 |
端口号 | 8080 |

### 安装
按照文件使用apt install安装  
安装后，会自动创建jenkins用户，并且以此用户运行jenkins  
运行的根目录在/var/lib/jenkins  
> 因为如果服务很多，会需要较大的空间，  
> 因此，如果使用云服务器，需要额外挂载一块较大磁盘  
> 可以将/var/lib/jenkins目录移动到新磁盘，创建软链接/var/lib/jenkins  

### 配置nginx
```
server {
    listen   80;
    server_name  jenkins.buxingxing.com;

    location / {
      proxy_pass http://127.0.0.1:8080/;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

### 登录
登录时会有提示查看密码:  
```
cat /var/lib/jenkins/secrets/initialAdminPassword
```
设置账号密码:  
```
账号: admin
密码: admin
```
