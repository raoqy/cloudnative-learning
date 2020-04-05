# Vagrant使用紀要
  

### **(一) 关键配置步骤 ：**
1. 使用vscode編輯vagrantfile文件，推荐的vscode 插件有哪些 ？
   
   Vagrant support for Visual Studio Code
   

2. [配置VM的网络]("https://www.vagrantup.com/docs/networking/public_network.html")
    * >[公有网络]("https://www.vagrantup.com/docs/networking/public_network.html")
      ```
        # 配置公开网络，关闭自动配置，设置桥接到本地的网络接口
        node.vm.network "public_network", auto_config: false, bridge: "Intel(R) Dual Band Wireless-AC 8265"

        # 通过shell脚本方式，配置静态IP
        node.vm.provision "shell",
          run: "always",
          inline: "ifconfig eth1 192.168.101.66 netmask 255.255.255.0 up"

        # 通过shell脚本方式，配置网关
        node.vm.provision "shell",
          run: "always",
          inline: "route add default gw 192.168.101.1"

        # 通过shell脚本方式，配置DNS
        node.vm.provision "shell",
          run: "always",
          inline: "route add default gw 192.168.101.1"

        # 通过shell脚本方式，指定脚本文件，配置网络
        node.vm.provision "shell",
          run: "always",
          paht: "setnetwork.sh"

      ```
    * >[私有网络]("https://www.vagrantup.com/docs/networking/private_network.html")
      ```

      ```

### **(二) 注意事項 ：**
>这是引用的内容
>>这是引用的内容
>>>>这是引用的内容

### **(三) FAQ :**
1. >vagrant up 启动后,会去自动下载安装一些库，很浪费时间，常常因此而导致脚本运行失败
   ```
    
   ```
   解决措施：可能是vagrant 的vagrant-vbguest插件和virtualbox的版本不兼容，可以把该插件去掉。大部分使用场景，问题不大
   
   ```
    vagrant plugin uninstall vagrant-vbguest
   ```

2. >vagrant up 启动后,继续报"Vagrant was unable to mount VirtualBox shared folders"
   ```
    Vagrant was unable to mount VirtualBox shared folders. This is usually
    because the filesystem "vboxsf" is not available. This filesystem is
    made available via the VirtualBox Guest Additions and kernel module.
    Please verify that these guest additions are properly installed in the
    guest. This is not a bug in Vagrant and is usually caused by a faulty
    Vagrant box. For context, the command attempted was:

    mount -t vboxsf -o uid=1000,gid=1000 vagrant /vagrant

    The error output from the command was:

    mount: unknown filesystem type 'vboxsf'

   ```
   解决措施：默认vagrant就回把当前目录挂载到虚拟机的/vagrant目录,注释vagrantfile中同步文件夹的代码，

   ```
    config.vm.synced_folder ".", "/vagrant", type: "nfs", nfs_udp: false

   ```

3. >vagrant up 启动后,继续报"No guest additions were detected on the base box for this VM"
   ```
    No guest additions were detected on the base box for this VM! Guest
    additions are required for forwarded ports, shared folders, host only
    networking, and more. If SSH fails on this machine, please install
    the guest additions and repackage the box to continue.
   ```
   解决措施：不用管它就行。