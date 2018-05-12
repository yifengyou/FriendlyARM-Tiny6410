# 第11课-TFTP与NFS服务器配置

交叉开发的需要，因此需要配置该服务。如何把宿主机的软件上传到目标机呢?因此用到了tftp和NFS
      宿主机：产生嵌入式软件，开发端
      目标机：宿主机产生的软件由目标机运行

![交叉开发](image/交叉开发.png)

![tftp和NFS模型](image/tftp和nfs模型.png)




* tftp

      红帽系统默认没有安装tftp，NFS默认有

![tftp服务器](image/tftp服务器.png)

![yum查找tftp和nfs](image/yum查找tftp和nfs.png)

![tftp安装](image/tftp安装.png)

      tftp默认配置，路劲在/etc/xinetd.d/tftp

![tftp默认配置](image/tftp默认配置.png)

![修改配置](image/修改配置.png)

      xinetd(extended internet daemon)是新一代的网络守护进程服务程序，又叫超级Internet服务器,常用来管理多种轻量级Internet服务。

![重启服务](image/重启服务.png)

      前提知道目标文件路径，完成文件下载
      TFTP 的端口号是69 port
          读取和写入请求
          RRQ (read request)
          WRQ (write request)
          皆采用 69 port
      需要注意的是，传送档案时并不是用69 port

![tftpclient](image/tftpclient.png)

* NFS

![nfs服务器](image/nfs服务器.png)

      指定共享目录
      赋予访问权限

![nfs服务器搭建](image/nfs服务器搭建.png)

      修改/etc/exports，默认为空文件。指定共享目录路径，其次，访问权限(指定用户或所有用户或网段用户，读写权限)
        no_root_squash：如果客户端是root用户登录，那么在服务器端也以root对待创建读写文件
        rw：读写权限
        sync：异步写
      一般只修改开头ip段

![指定网段共享](image/指定网段共享.png)

~~~
/tmp 10.59.13.*(rw,sync,no_root_squash)
~~~

![重启服务](image/重启服务.png)
