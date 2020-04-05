# Mysql数据库主从集群的容器化实践

## 基于helm 安裝mysql集群

1. 部署nfs storageclass
   
2. 部署mysql helm chart 
   ```
   # 修改chart的value.yaml 
     vi mysql/value.yaml

   # 安装bitnami/mysql helm chart
   helm install -f values.yaml  mysql-cluster bitnami/mysql --generate-name

   # 验证一下mysql的可用性
   
   ```

3. 验证一下mysql集群的可用性
   ```
   NOTES:
   Please be patient while the chart is being deployed

   Tip:

    Watch the deployment status using the command: kubectl get pods -w --namespace default

   Services:

    echo Master: mysql-cluster.default.svc.cluster.local:3306
    echo Slave:  mysql-cluster-slave.default.svc.cluster.local:3306

    Administrator credentials:

    echo Username: root
    echo Password : $(kubectl get secret --namespace default mysql-cluster -o jsonpath="{.data.mysql-root-password}" | base64 --decode)

    To connect to your database:

    1. Run a pod that you can use as a client:

        kubectl run mysql-cluster-client --rm --tty -i --restart='Never' --image  docker.io/bitnami/mysql:8.0.19-debian-10-r64 --namespace default --command -- bash

    2. To connect to master service (read/write):

        mysql -h mysql-cluster.default.svc.cluster.local -uroot -p my_database

    3. To connect to slave service (read-only):

        mysql -h mysql-cluster-slave.default.svc.cluster.local -uroot -p my_database

    To upgrade this helm chart:

    Obtain the password as described on the 'Administrator credentials' section and set the 'root.password' parameter as shown below:

        ROOT_PASSWORD=$(kubectl get secret --namespace default mysql-cluster -o jsonpath="{.data.mysql-root-password}" | base64 --decode)
        helm upgrade mysql-cluster bitnami/mysql --set root.password=$ROOT_PASSWORD

   master password: rGp42uN58w

   ```