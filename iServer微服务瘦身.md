在公司外网下载镜像
docker pull registry.ispeco.com/supermap/iserver-datascience:latest

docker tag registry.ispeco.com/supermap/iserver-datascience:latest 镜像名:标签

上传镜像到阿里云
镜像名 registry.cn-beijing.aliyuncs.com/supermap/iserver-datascience

sudo docker login --username=laimei@1517438161519050 registry.cn-beijing.aliyuncs.com

docker push registry.cn-beijing.aliyuncs.com/supermap/iserver-datascience:标签

上传到 docker hub 
镜像名 supermap/iserver-datascience 

帐号supermap

docker login

docker push supermap/iserver-datascience:标签



登陆阿里云

https://account.aliyun.com/login/login.htm?oauth_callback=https%3A%2F%2Fcr.console.aliyun.com%2Fcn-beijing%2Finstances%2Fimages

账户:

laimei@1517438161519050.onaliyun.com

选择 华北2

镜像仓库

搜索 iserver-datascience 

查看是否上传成功

更新示例站点
通知许工更新datascience站点

镜像名 supermap/iserver-datascience