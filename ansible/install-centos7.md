#### Ansible安装
---
**注意**：ansible安装多种方法，rpm安装、yaml安装、二进制安装、git源码安装、pip安装等

linux - centos7安装ansible

```SH
[root@master ~]# cat /etc/redhat-release
CentOS Linux release 7.9.2009 (Core)
```

实验环境：

| 角色            | 主机IP         |
| --------------- | -------------- |
| 主控端 master   | 192.168.31.201 |
| 控制节点 node01 | 192.168.31.202 |
| 控制节点 node02 | 192.168.31.203 |

python：3.6版本

```SH
[root@master ~]# python3
Python 3.6.8 (default, Nov 16 2020, 16:55:22)
```



官方文档：https://cn-ansibledoc.readthedocs.io/zh_CN/latest/installation_guide/intro_installation.html#ansible-on-rhel-centos-or-fedora

```SH
$ yum install epel-release
$ yum repolist
$ yum info ansible
$ sudo yum install ansible

#安装后生成的文件列表：
[root@master ~]# rpm -ql ansible
/etc/ansible
/etc/ansible/ansible.cfg
/etc/ansible/hosts
/etc/ansible/roles
/usr/bin/ansible
...
```

**文件：**

+ 主配置文件：/etc/ansible/ansible.cfg （大部分无需修改）
+ 主机清单：/etc/ansible/hosts
+ 存放角色目录：/etc/ansible/roles

git安装

```SH
$ git clone git://github.com/ansible/ansible.git --recursive #将项目源码拷到本地主机
$ cd ./ansible
$ source ./hacking/env-setup #使用bash安装

##更新版本方法
##更新ansible版本时,不只要更新git的源码树,也要更新git中指向Ansible自身模块的 “submodules” (不是同一种模块)
$ git pull --rebase
$ git submodule update --init --recursive
#然后再次运行env-setup脚本启动即可
```

#### 2.1.4 Ansible命令自动补全 ####

​	Ansible 从 2.9 版本开始支持命令行补全功能，但需要安装 `argcomplete` 插件。 `argcomplete` 完全支持 bash， 部分功能支持 zsh tcsh。

RedHat 的 EPEL 源直接安装 `python-argcomplete`

```SH
yum install python-argcomplete -y
```

`bash -version `:大于等于4.2

激活:`python-argcomplete`

```SH
activate-global-python-argcomplete
```

注意：关闭bash窗口重新登录，生效

