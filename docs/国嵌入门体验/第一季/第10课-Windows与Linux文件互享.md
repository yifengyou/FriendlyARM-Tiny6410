# 第10课-Windows与Linux文件互享

* xftp
      为方便实验关闭防火墙，检测是否可以ping通主机。检测主机ip

![dhclient获取ip](image/dhclient获取ip.png)

      使用Xmanager的xftp通过SSH登录到虚拟机，即可操作文件

![xftp登录虚拟机](image/xftp登录虚拟机.png)

![xftp](image/xftp.png)

* 使用Xshell终端通过lrzsz上传下载单个文件(不支持目录)

      sz 下载 rz 上传

![lrzsz](image/lrzsz.png)

![sz](image/sz.png)

![rz](image/rz.png)

* samba

![yum安装samba](image/yum安装samba.png)

![samba安装](image/samba安装.png)

![samba常见问题](image/samba常见问题.png)

![useradd_samba](image/useradd_samba.png)

![smbpasswd](image/smbpasswd.png)

      查找home配置，复制该配置，修改其内容，允许访问，可写，添加默认访问路径，添加有效用户

![smbconfig](image/smbconfig.png)

![smb协议过期](image/smb协议过期.png)

      输入的用户名密码是Linux创建的用户和smbpasswd设定的用户名密码，为了区别于samba密码和用户密码

![使用xp访问samba](image/使用xp访问samba.png)

![xp访问samba](image/xp访问samba.png)

      SeLinux是Linux的安全模块。普通samba用户无法正常读写文件。

![samba普通用户无法访问root目录](image/samba普通用户无法访问root目录.png)

![关闭selinux](image/关闭selinux.png)

* VMware tools

      不建议使用
