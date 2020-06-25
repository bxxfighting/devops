### 删除自带的maven库
```
maven-central
maven-public
maven-releases
maven-snapshots
nuget-group
nuget-hosted
nuget.org-proxy
```
> 删除自带的7个repo，因为这里只是当成了制品库，不是做java的maven库，所以这些都删除掉  

### 创建不同环境的repo
创建qa环境的repo:  
Create repository -> maven2(hosted)  
创建信息：  
Name: qa  

创建release环境的repo:  
Create repository -> maven2(hosted)  
创建信息：  
Name: release  

创建生产环境的repo:  
Create repository -> maven2(hosted)  
创建信息：  
Name: prod  

> 这里只是示例，根据实际情况创建制品库  
