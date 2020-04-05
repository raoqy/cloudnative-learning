# 基于vagrant+k8s+helm+nfs搭建测试环境

## (一) vagrant 搭建Kubeernetes踩过的坑

### vagrant使用过程中遇到的一些问题及解决方案

### docker镜像拉取不下来，原因何在 ？ 
1. ping 海外的几个docker镜像地址，失败
2. 需要添加国内的docker镜像地址，可以添加163，教育网之类的镜像地址
3. 修改/etc/docker/daemon.json,添加如下内容：
   ```
   {
    "registry-mirrors" : [
    "https://dockerhub.azk8s.cn",
    "https://hub-mirror.c.163.com",
    "http://2595fda0.m.daocloud.io"
    ]
   }
   
   ```
4. 再拉取某个镜像，还是不行，拉不下来，控制台报这种错
5. 排查原因，发现是dns的问题，53端口是dns服务的端口
   ```
     Trying to pull repository quay.io/external_storage/nfs-client-provisioner ... 
     Get https://quay.io/v1/_ping: dial tcp: lookup quay.io on 10.0.2.3:53: read udp 10.0.2.15:41684->10.0.2.3:53: i/o timeout
   ```
6. 需要修改dns设置，补充更多可用的dns地址
7. 通过nmcli 命令来设置dns,具体如下：
   ```
     # 显示所有的设备
     nmcli con show
     # 修改某个设备的dns,通过uuid来指定设备
     nmcli con mod xxxx ipv4.dns '114.114.114.114,8.8.8.8'
     # 重新启动某设备
     nmcli con up xxx
     
   ```
 1. 再次拉取docker镜像，发现成功了


## (二) 配置NFS StorageClass

### 1. 在CentOS7系统上启动NFS服务

   ```
     # 启动nfs相关系统服务
     systemctl start rpcbind
     systemctl start nfs

     # 设置开机自启动
     systemctl enable rpcbind
     systemctl enable nfs
 
     # 设置发布目录
     vi /etc/exports
        /home/nfs  *192.168.0.0/24*(rw,sync,all_squash,anonuid=1001,anongid=1001)
     
     # 检查发布目录
     showmount -e
        /home/nfs *

   ```


### 2. 通过helm 安装nfs-client-provisioner 
   ```
     # 搜索helm仓库，看看nfs相关chart具体有哪些 ？ 
     helm search repo nfs 
     
     # 修改value.yaml文件，设置nfs连接信息
     helm install -f ./value.yaml stable/nfs-client-provisioner --generate-name
     
     # 检查已安装成功的helm chart 
     helm list 
       
        NAME                             	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART                       	APP VERSION
            
        nfs-client-provisioner-1584272596	default  	1       	2020-03-15 19:43:21.573495936 +0800 CST	deployed	nfs-client-provisioner-1.2.8	3.1.0
        
   ```
 



   
