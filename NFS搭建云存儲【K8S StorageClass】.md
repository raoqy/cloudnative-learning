# NFS搭建云存儲【K8S StorageClass】

## 安装部署nfs-client-provisioner helm chart 

1. 修改所有k8s节点的docker镜像设置 
2. 添加helm的官方仓库
   ```
   helm repo add stable https://kubernetes-charts.storage.googleapis.com/
   
   ```

3. 安装helm chart 
   ```
   helm install --name nfs-client --set nfs.server=192.168.101.101 --set nfs.path=/opt/nfsexports stable/nfs-client-provisioner
    
   ```