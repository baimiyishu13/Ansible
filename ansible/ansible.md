# 运维自动化ansible #

[toc]

### 1.1 应用场景 ###

用于部署复杂的程序，一个自动化工具。

简述：

​	ansible主要为三点：

1. ansible常见模块使用：可以像Linux命令一样，相当于一个工具集，每个子命令相当于一个模块（管理账号、文件、包管理等模块），实现不同模块，目前已经提供了上千个。
2. ansible脚本：ansible playbook(剧本) 是yaml语言实现。实现变量、条件判断、循环等。可以理解为换了形式的脚本。可以组合脚本，避免单个脚本功能限制。
3. ansible roles：角色，调用多个剧本组合实现相对复杂；如ansible部署



#### 1.1.1 高级工程师核心技能 ####

1. 平台架构组建
2. 日常运营保障：稳定运行、出现问题快速定位解决
3. 性能、效率优化：自动化的工具/平台 确保在复杂的条件下，同时保持高可用性

计划、编码、打包、测试、发布、部署、运维、监控

相关工具

+ 代码管理：GitHub、GitLab
+ 构建工具：maven（java）
+ 自动部署：*
+ 持续集成（CI）：Jenkins
+ 配置管理：ansible
+ 容器：docker
+ 编排：kubernetes
+ 服务注册与发现：kafka、Zookeeper、etcd
+ 脚本：python、shell
+ 日志：ELK
+ 监控：Prometheus、Zabbix
+ 性能监控：*
+ 压力测试：*
+ 应用服务器：Tomcat
+ web服务器：apach、Nginx
+ 项目管理（PM）：*



#### 1.1.2 常用的自动化工具 ####

+ ansible：python开发（无代理），中小型应用环境
+ Saltstack：python开发，一般需要部署agent，执行效率更高（有代理）



### 2.1 Ansible 安装&基本使用

#### 2.1.1 Ansible 介绍 & 组成 ####

 ![ansible Logo](image/image-20221023082257931.png)
 

官方：https://www.ansible.com/

文档：https://docs.ansible.com/

中文：https://cn-ansibledoc.readthedocs.io/zh_CN/latest/user_guide/index.html



##### 2.1.1.1 介绍 #####

+ 模块化：调用特定的模块，完成特定
+ Paramiko （python对ssh的实现），PyYAML，Jinja2（模板语言）三个关键模块
+ 支持自定义模块，可使用如何编程语言写模块 
+ 基于python语言实现
+ 部署简单，基于puyhon和SSH（默认已经安装），agentless，无需代理不依赖PKI（无需ssl）
+ 安全，基于OpenSSH
+ 幂等性：一个任务执行一次和n次效果一样，不因重复执行带来意外情况
+ 支持playbook编排任务，YAML格式，编排任务，支持丰富的数据结构
+ 较强大的多层解决方案role

##### 2.1.1.2 组成 #####

主要关注：管理Linux系统

主控端（master）：执行ansible命令的主机；安装了 Ansbile 的机器都可以做为管理节点

受控节点：Ansbile 管理的服务器或者网络设备都称为受控节点。 

Inventory 仓库：保存受控节点信息的列表, 因为有时候也叫 “hostfile”， 类似于系统的 hosts 文件。 Inventory 仓库可以以 IP 的方式指定受控节点。

Playbooks（局部）： 是任务列表的组合，通常会把常用的命令列表通过正确的语法写入到 playbook中



#### 2.1.2 Ansible安装 ####

##### 2.1.2.1 安装Ansible #####

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

##### 2.1.2.2Ansible 命令自动补全 #####

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



#### 2.1.3 Ansible 相关配置文件

##### 2.1.3.1 重要文件 #####

主机清单文件：/etc/ansible/hosts

主配置文件：/etc/ansible/ansible.cfg

**注：**注释到的为默认值

```SH
[defaults]
#inventory      = /etc/ansible/hosts 					#主机列表
#library        = /usr/share/my_modules/				#库存放目录
#module_utils   = /usr/share/my_module_utils/			 
#remote_tmp     = ~/.ansible/tmp						#临时py命令文件存放再远程目录录
#local_tmp      = ~/.ansible/tmp						# 本机的临时命令执行目
#plugin_filters_cfg = /etc/ansible/plugin_filters.yml
#forks          = 5		# 并发
#poll_interval  = 15	
#sudo_user      = root 	# 默认sudo用户
#ask_sudo_pass = True	# 每次执行ansible命令是否询问ssh密码
#ask_pass      = True
#transport      = smart
#remote_port    = 22
#module_lang    = C
#module_set_locale = False
host_key_checking = False # 建议取消注释
log_path = /var/log/ansible.log	#日志文件
#module_name = command	#默认模块,可改为shell
```

##### 2.1.3.2 inventory 主机清单 #####

ansible的主要功能在于批量主机操作，为了便捷地实验其中部分主机，可以在inventory file中将其分组命名默认的inventory file 为/etc/ansible/hosts 

inventory file可以有多个，可以从动态源,或云上拉取 inventory 配置信息



最简单的方式：将主机IP都写上去！！

标签：机器数量多时，维护就很麻烦，所以需要分类，写标签页 “[ ]”

例如：

```SH
[webservers]
192.168.31.[201:203]

[dbserveres]
192.168.31.201
```

#### 2.1.4 ansible相关工具 ####

+ ansible：主程序，临时命令执行
+ ansible-doc：查看文档
+ ansible-galaxy：下载上传代码 或 roles模块的官方平台
+ ansible-playbook：定制自动化任务，编排剧本工具
+ ansible-pull：远程执行命令的工具
+ ansible-vault：文件加密工具
+ ansible-console：基于console解密与用户交互的执行工具

**利用ansible实现管理的主要方式：**

+ Ad-Hoc 即利用ansible命令，主要用于零时命令使用场景
+ Ansible-playbook：主要用于长期规划好的，大型项目场景，需要有前提的规划过程

##### 2.1.4.1  ansible-doc #####

此工具用来显示模块帮助

```SH
[root@master ~]# ansible-doc -l | wc
   3387   25255  396357
```

用法：

```SH
$ ansible-doc [-h] [--version] [-v] [-M MODULE_PATH]
```

查看指定模块帮助方法：例如ping

```SH
[root@master ~]# ansible-doc ping
[root@master ~]# ansible-doc -s ping
- name: Try to connect to host, verify a usable python and return `pong' on success
  ping:
      data:                  # Data to return for the `ping' return value. If this parameter is set to `crash',
                               the module will cause an exception.
You have new mail in /var/spool/mail/root
```

**重要：**ansible `-s`: 如何为指定的插件编写剧本片段

