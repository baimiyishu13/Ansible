# 运维自动化ansible #
- [运维自动化ansible](#运维自动化ansible)
    - [1.1 应用场景](#11-应用场景)
      - [1.1.1 高级工程师核心技能](#111-高级工程师核心技能)
      - [1.1.2 常用的自动化工具](#112-常用的自动化工具)
    - [2.1 Ansible 安装&基本使用](#21-ansible-安装基本使用)
      - [2.1.1 Ansible 介绍 & 组成](#211-ansible-介绍--组成)
        - [2.1.1.1 介绍](#2111-介绍)
        - [2.1.1.2 组成](#2112-组成)
      - [2.1.2 Ansible安装](#212-ansible安装)
        - [2.1.2.1 安装Ansible](#2121-安装ansible)
        - [2.1.2.2Ansible 命令自动补全](#2122ansible-命令自动补全)
      - [2.1.3 Ansible 相关配置文件](#213-ansible-相关配置文件)
        - [2.1.3.1 重要文件](#2131-重要文件)
        - [2.1.3.2 inventory 主机清单](#2132-inventory-主机清单)
      - [2.1.4 ansible相关工具](#214-ansible相关工具)
        - [2.1.4.1  ansible-doc](#2141--ansible-doc)
        - [2.1.4.2 ansible命令](#2142-ansible命令)
        - [2.1.4.3 ansible-galaxy](#2143-ansible-galaxy)
        - [2.1.4.4 ansible-pull](#2144-ansible-pull)
        - [2.1.4.5 ansible-playbook](#2145-ansible-playbook)
        - [2.1.4.6 ansible-vault](#2146-ansible-vault)
        - [2.1.4.7 ansible-console](#2147-ansible-console)
    - [3.1 Ansible常用模块详解](#31-ansible常用模块详解)
      - [3.1.1 Command](#311-command)
      - [3.1.2 shell模块](#312-shell模块)
      - [3.1.2 Script模块](#312-script模块)
      - [3.1.3 Copy模块](#313-copy模块)
      - [3.1.4 Fetch模块](#314-fetch模块)
      - [3.1.5 File模块](#315-file模块)
      - [3.1.6 unarchive模块](#316-unarchive模块)
      - [3.1.7 Archive模块](#317-archive模块)
      - [3.1.8 Hostname模块](#318-hostname模块)
      - [3.1.9 Cron模块](#319-cron模块)
      - [3.1.10 Yum 模块](#3110-yum-模块)
      - [3.1.11 Service 模块](#3111-service-模块)
      - [3.1.12 User模块](#3112-user模块)
      - [3.1.12 Group模块](#3112-group模块)
      - [3.1.13 Lineinfie模块](#3113-lineinfie模块)
      - [3.4.16 replace模块](#3416-replace模块)
      - [3.4.17 Setup 模块](#3417-setup-模块)
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

 ![ansible Logo](../images/image-20221023082257931.png)
 

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

##### 2.1.4.2 ansible命令 #####

此工具通过ssh协议，实现对远程主机的配置管理，应用部署，任务执行等功能

建议：先配置ansible主控端基于密钥认证连接到各个被控节点

如下：利用ssh批量实现基于key认证

```SH
/etc/hosts
192.168.31.201 master
192.168.31.202 node01
192.168.31.203 node02

[root@master ~]# ssh-keygen
[root@master ~]# for i in node01 node02;do ssh-copy-id -i .ssh/id_rsa.pub $i;done
[root@master ~]# ssh node02
Last login: Sun Oct 23 09:22:45 2022 from 192.168.31.20
[root@node02 ~]# exit
logout
Connection to node02 closed.
```

 测试：`ping`

```SH
[root@master ~]# ansible all -m ping
192.168.31.203 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
192.168.31.202 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
[root@master ~]# ansible all --list
  hosts (2):
    192.168.31.202
    192.168.31.203
```

注意：

+ all：表示所有主机
+ all 可以写成分组名称 appservers、*、地址段、关系或、逻辑与、逻辑非、正则表达式等

```SH
[root@master ~]# ansible appservers --list-hosts
  hosts (2):
    192.168.31.202
    192.168.31.203
[root@master ~]# ansible "app:&db" -m ping
192.168.31.202 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
[root@master ~]# ansible 'app:!db' -m ping			# **注意引号：逻辑非**

192.168.31.203 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
```

ansible的执行过程：

`-v/-vvv:`查看ansible执行的详细过程

```SH
<192.168.31.202> (0, '\r\n{"invocation": {"module_args": {"data": "pong"}}, "ping": "pong"}\r\n', 'Shared connection to 192.168.31.202 closed.\r\n')
<192.168.31.202> ESTABLISH SSH CONNECTION FOR USER: None
<192.168.31.202> SSH: EXEC ssh -C -o ControlMaster=auto -o ControlPersist=60s -o StrictHostKeyChecking=no -o KbdInteractiveAuthentication=no -o PreferredAuthentications=gssapi-with-mic,gssapi-keyex,hostbased,publickey -o PasswordAuthentication=no -o ConnectTimeout=10 -o ControlPath=/root/.ansible/cp/b5166bec1d 192.168.31.202 '/bin/sh -c '"'"'rm -f -r /root/.ansible/tmp/ansible-tmp-1666508088.25-19549-185664419358812/ > /dev/null 2>&1 && sleep 0'"'"''
<192.168.31.202> (0, '', '')
192.168.31.202 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "invocation": {
        "module_args": {
            "data": "pong"
        }
    },
    "ping": "pong"
}

```

1. 加载自己的配置文件，默认是/etc/ansible/hosts
2. 加载自己对应的模块文件，如：command
3. 通过ansible将模块或命令生成对应的临时文件，并将该文件传输至远程服务器的对应执行用户 $Home/.ansible/tmp/ansible-tmp-**/xxx.py
4. 给文件+x权限执行
5. 执行并返回结果
6. 删除临时py文件，退出



##### 2.1.4.3 ansible-galaxy #####

官方文档：https://cn-ansibledoc.readthedocs.io/zh_CN/latest/galaxy/user_guide.html

此工具会连接https://galaxy.ansible.com/ 下载相应的roles

范例：

```SH
$ ansible-galaxy list # 列出所有已安装的galaxy
$ ansible-galaxy remove namespace.role_name #删除
```

下载：playbook的集合

```SH
$ansible-galaxy collection install community.mysql

[root@master .ansible]# ansible-galaxy collection install community.mysql
Process install dependency map
Starting collection install process
Installing 'community.mysql:3.5.1' to '/root/.ansible/collections/ansible_collections/community/mysql'
```

##### 2.1.4.4 ansible-pull #####

此工具会推送ansile的命令值远程，效率无限提升，对运维要求高。

基本上很少用



##### 2.1.4.5 ansible-playbook #####

此工具用于执行编写好的playbook任务

范例：

注：wall命令：传讯息"***" 给每一个使用者

```yaml
- hosts: app
  remote_user: root
  tasks:
    - name: hello world
      command: /usr/bin/wall hello world
```

```SH
[root@master playbook]# ansible-playbook hello.yaml

PLAY [app] **********************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************
ok: [192.168.31.203]
ok: [192.168.31.202]

TASK [hello world] **************************************************************************************************
changed: [192.168.31.203]
changed: [192.168.31.202]

PLAY RECAP **********************************************************************************************************
192.168.31.202             : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
192.168.31.203             : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

##### 2.1.4.6 ansible-vault #####

保护playbook的安全

此工具可以用于加密解密yaml文件

格式：

```SH
[root@master playbook]# ansible-vault encrypt hello.yaml
New Vault password:
Confirm New Vault password:
Encryption successful
[root@master playbook]# ansible-playbook hello.yaml
ERROR! Attempting to decrypt but no vault secrets found
[root@master playbook]# cat hello.yaml
$ANSIBLE_VAULT;1.1;AES256
38623036363632663137343136643035356362383563643434366132643162373339383265323631
6339613163666661386236383339353837353430626133350a306239396636646439616134366332
39643331326662386431303863666563343064666437663530623939303937326661643138306166
3835623332663232370a616561626130313030336666613330323233393163653665333534373139
65343034306364633837613765383066643936643437393865623463353165633566656336393339
64396434373665633330643637313136363764313034323661353165653462306635616435356634
61336434656538633335633433376537663863343530666664636632663636666536353862636137
31633662303938393730353534363337366463313332613462356531656364626566366338633038
33336438613138363063373335303362326430323165346437616262353933313339
```

解密才能执行：

```SH
[root@master playbook]# ansible-vault decrypt hello.yaml
Vault password:
Decryption successful
[root@master playbook]# cat hello.yaml
- hosts: app
  remote_user: root
  tasks:
    - name: hello world
      command: /usr/bin/wall hello world
```

##### 2.1.4.7 ansible-console #####

此工具可交互式执行命令，支持tab，ansible 2.0+新增

提示符格式：`执行用户@当前操作的主机组（当前组的主机数量）[f:并发数]$`

```SH
[root@master ~]# ansible-console
Welcome to the ansible console.
Type help or ? to list commands.

root@all (2)[f:5]$ 
```

常用子命令：

+ 设置并发数：forks n 例如：forks10
+ 切换组：cd 主机组，例如：cd db
+ 列出当前主机组列表：list
+ 列出所有的内置命令：？或help

示例：

```sh
root@db (1)[f:5]$ cd all
root@all (2)[f:5]$
root@all (2)[f:5]$ list
192.168.31.202
192.168.31.203
root@all (2)[f:5]$ cd db
root@db (1)[f:5]$ list
192.168.31.202
root@db (1)[f:5]$ yum name=httpd state=present
...
root@db (1)[f:5]$ service name=httpd state=started
....
```

192.168.31.202查看httpd状态：`active (running)`


### 3.1 Ansible常用模块详解 ###

ansible：上千个模块

虽然模块众多，但是常用的也就几十个而已，正对特定业务只用1-几个模块



#### 3.1.1 Command ####

`默认模块`

功能：再远程主机执行命令，此为默认模块，可忽略 `-m`选项

-m：指定模块

-a：模块的参数

注意：command模块的linux命令存在局限性，并发所有linux都可以成功执行！！

```SH
[root@master ~]# ansible-doc -s command
- name: Execute commands on targets
  command:
      argv:                  # Passes the command as a list rather than a string. Use `argv' to avoid quoting
                               values that would otherwise be interpreted
                               incorrectly (for example "user name"). Only the
                               string or the list form can be provided, not both.
                               One or the other must be provided.
      chdir:                 # Change into this directory before running the command. #在运行命令之前西先到到此目录。
      cmd:                   # The command to run.
      creates:               # A filename or (since 2.0) glob pattern. If it already exists, this step *won't* be
                               run.
      free_form:             # The command module takes a free form command to run. There is no actual parameter
                               named 'free form'.
      removes:               # A filename or (since 2.0) glob pattern. If it already exists, this step *will* be
                               run.
      stdin:                 # Set the stdin of the command directly to the specified value.
      stdin_add_newline:     # If set to `yes', append a newline to stdin data.
      strip_empty_ends:      # Strip empty lines from the end of stdout/stderr in result.
      warn:                  # Enable or disable task warnings.
```

注意：

+ creates 和 removes：如根据文件是否存在判断是否执行某个操作，creates存在跳过， removes存在执行

示例：

```SH
[root@master ~]# ansible db -m command -a 'systemctl start docker'
192.168.31.202 | CHANGED | rc=0 >>

[root@master ~]# ansible db -m command -a 'pwd'
192.168.31.202 | CHANGED | rc=0 >>
/root
[root@master ~]# ansible db -a 'pwd'  # command 默认模块
192.168.31.202 | CHANGED | rc=0 >>
/root

[root@master ~]# ansible db -m command -a 'chdir=/etc pwd'	
192.168.31.202 | CHANGED | rc=0 >>
/etc
[root@master ~]# ansible db -m command -a 'creates=/opt pwd'
192.168.31.202 | SUCCESS | rc=0 >>
skipped, since /opt exists
[root@master ~]# ansible db -m command -a 'removes=/opt pwd'
192.168.31.202 | CHANGED | rc=0 >>
/root
```

shell模块：为了解决command不支持很多特殊符号的问题

#### 3.1.2 shell模块 ####

功能：和command相似，用shell执行命令

示例：

```bash
[root@master ~]# ansible db -m shell -a 'echo $HOSTNAME'
192.168.31.202 | CHANGED | rc=0 >>
node01


[root@master ~]# ansible db -m shell -a 'echo hello > /opt/1.txt'
192.168.31.202 | CHANGED | rc=0 >>

[root@master ~]# ansible db -m shell -a 'cat /opt/1.txt'
192.168.31.202 | CHANGED | rc=0 >>
hello
```

注意：调用bash执行命令，类似awk这些复杂的命令，即使调用shell 也可能会失败，解决办法：写道脚本copy到远程执行，再将结果拉回。

#### 3.1.2 Script模块 ####

功能：远程主机上运行ansible服务器上的脚本

```SH
#!/bin/sh
hostname
```

```SH
 ansible db -m script -a script/test.sh
```

注意：shell 模块执行命令前提条件：目标已经存放再主机

```SH
[root@master ~]# scp script/test.sh node01:/opt
test.sh                                                                          100%   19    12.4KB/s   00:00
[root@master ~]# ansible db -m shell -a 'chdir=/opt chmod +x test.sh&&sh test.sh'
192.168.31.202 | CHANGED | rc=0 >>
node01
```



script优势：不需要将脚本拷贝到被控节点



#### 3.1.3 Copy模块 ####

功能：从ansible服务器主控端复制文件到远程主机

backup：yes/no ：备份

**（1）备份**

如果目标存在，默认覆盖（`覆盖：高危命令`），先指定备份

```SH
[root@master ~]# ansible db -m shell -a 'cat /tmp/test'
192.168.31.202 | CHANGED | rc=0 >>
test copy

echo test copy backup > test
ansible db -m copy -a 'src=/root/test dest=/tmp/test backup=yes'

[root@master ~]# ansible db -m shell -a 'ls /tmp | grep test'
192.168.31.202 | CHANGED | rc=0 >>
test
test.109562.2022-10-23@19:18:07~		# 备份文件
```

**（2）指定内容**

content ：直接指定内容

```SH
ansible db -m copy -a "content='test content\n' dest=/tmp/test2"

[root@master ~]# ansible db -m shell -a 'cat /tmp/test2'
192.168.31.202 | CHANGED | rc=0 >>
test content
```

**（3）复制目录下文件**

复制目录下的文件，不包括目录本身

```SH
ansible db -m copy -a 'src=script/ dest=/backup'

[root@master ~]# ansible db -m shell -a 'ls /backup'
192.168.31.202 | CHANGED | rc=0 >>
test.sh
```

#### 3.1.4 Fetch模块 ####

功能：从远端提取文件至ansible主控端，与copy相反，目前不支持目录

范例：

```SH
ansible all -m fetch -a 'src=/etc/redhat-release dest=/data/'
```

生成了子目录

```SH
[root@master ~]# tree /data/
/data/
├── 192.168.31.202
│   └── etc
│       └── redhat-release
└── 192.168.31.203
    └── etc
        └── redhat-release
```

#### 3.1.5 File模块 ####

功能：设置文件文件属性

状态：state

+ touch：生成空文件
+ abset：递归删除（删除）
+ directory：目录

path：必选

recurse：递归创建目录，state必须是directory

示例：

```SH
 ansible db -m file -a 'path=/data state=directory'
 
# 创建空文件
 ansible db -m file -a 'path=/data/test.txt state=touch'
 
# 删除文件
 ansible db -m file -a 'path=/data/test.txt state=absent'

# 创建目录
 ansible db -m shell -a 'useradd mysql'
 ansible db -m file -a 'path=/data/mysql state=directory recurse=true owner=mysql group=mysql'
 
 # 创建软链接
 ansible db -m file -a 'path=/data/testfile dest=/data/testfile-link state=link'
```

#### 3.1.6 unarchive模块 ####

功能：解包\解压包

实现方法：（2种）

1. 将ansible 主机上的压缩包传到远程被控节点后解压缩到特定目录，设置`COPY=yus`
2. 将远程主机上的某个压缩包解压到指定路径下，设置`copy=no`

常见参数

+ `COPY`：默认yes，拷贝的文件时从ansible主机上复制到 被控节点。如果`copy=no` 会在远程主机上寻找src源文件
+ `remote_src`：和copy功能一样且互斥，yes表示在被控节点，不在主控端。no表示文件在主控端
+ `src`：源路径，可以在主控端和被控节点，如果在被控节点，则需要设置`copy=no`
+ `dest`：被控节点的目标路径
+ `mode`：设置解压缩的文件权限

示例：

```SH
[root@master ~]# tar -cvf data.tar /data

ansible db -m unarchive -a 'src=/root/data.tar dest=/root/test'
```

被控节点：压缩包

```SH
[root@node01 ~]# tar -zcvf test/data.tar.gz test/data/
[root@node01 ~]# ls test/
data  data.tar.gz

# 主控端
ansible db -m unarchive -a 'src=/root/test/data.tar.gz dest=/root/test2 copy=no'
```

**remote_src &mode**

```SH
ansible db -m unarchive -a 'src=/root/test/data.tar.gz dest=/root/test remote_src=yes mode=755'
```

注意：打包功能直接使用copy拷贝到被控节点！



#### 3.1.7 Archive模块 ####

功能：打包压缩

示例：

```SH
ansible db -m archive -a 'path=/root/test/test/data dest=/root/data.tar.gz format=gz'
```

#### 3.1.8 Hostname模块 ####

功能：管理主机名

```SH
ansible db -m hostname -a 'name=dbsrc'
[root@master ~]# ansible db -m shell -a 'cat /etc/hostname'
192.168.31.202 | CHANGED | rc=0 >>
dbsrc
```

#### 3.1.9 Cron模块 ####

功能：计划任务

支持时间：分、时、日、月、周

minute、hour、day、month、weekday

**重要数据备份脚本**

```SH
# 被控节点
[root@node01 ~]# mkdir console
[root@node01 ~]# cd console/
[root@node01 console]# touch index.thml
```

需要备份 config下的文件 周1-5，每个2小时30分

```SH
[root@node01 ~]# cat console-backup.sh
#!/bin/sh
cp /root/console/index.html /root/console/index-`date +%F-%k-%M-%S`.html &>/dev/null
[root@node01 ~]# chmod a+x console-backup.sh
```

测试：每分钟

```SH
# 主控端
ansible db -m cron -a 'minute=*/1 job=/root/console-backup.sh name=backup name=backup-console'

# 被控节点
[root@node01 console]# ll
total 0
-rw-r--r-- 1 root root 0 Oct 23 22:03 index-2022-10-23-22-03-01.html
-rw-r--r-- 1 root root 0 Oct 23 22:04 index-2022-10-23-22-04-01.html
-rw-r--r-- 1 root root 0 Oct 23 21:57 index.htm
```

`disabled=yes`：禁止任务计划（注释掉），启用就是 no

```SH
 ansible db -m cron -a 'minute=*/1 job=/root/console-backup.sh name=backup name=backup-console disabled=yes'
```

查看：`crontab -l`

```SH
[root@master ~]# ansible db -m shell -a 'crontab -l'                                                 
192.168.31.202 | CHANGED | rc=0 >>
*/5 * * * * /usr/sbin/ntpdate time2.aliyun.com
#Ansible: backup-console
#*/1 * * * * /root/console-backup.sh
```

删除任务计划：

```SH
ansible db -m cron -a 'name=backup-console state=absent'
```

#### 3.1.10 Yum 模块 ####

功能：管理软件包,支持RHEL、Centos、Fedora

状态state：

+ absent：删除
+ latest：安装最新
+ present确保存在(安装)

示例：

```SH
ansible db -m yum -a 'name=httpd state=present'
ansible db -m yum -a 'name=httpd state=latest'
ansible db -m yum -a 'name=httpd state=absent'
```

#### 3.1.11 Service 模块 ####

功能：管理服务

状态：state

+ started：启动
+ stopped：暂停
+ restarted：重启
+ reloaded：重装
+ enabled：开机启动 enables=yes

示例：

```SH
ansible db -m service -a 'name=httpd state=restarted'
ansible db -m service -a 'name=httpd enabled=yes'
```

#### 3.1.12 User模块 ####

功能：管理用户

示例：

+ name：用户名
+ comment：描述
+ uid: 1040
+ group: 用户的主组
+ groups: 用户的附加组
+ remove：删除用户
+ create_home：要不要创建home目录
+ home：指定用户的home目录

```SH
ansible db -m user -a 'name=johnd comment="John Doe" uid=1040 group=root'
ansible db -m user -a "name=user1 comment='test user' uid=1042 home=/app/user1 group=root"

# 删除
ansible db -m user -a 'name=user1 state=absent remove=yes'
```

#### 3.1.12 Group模块 ####

功能：管理组

state：present:创建,absent:删除

system：创建系统组(几乎用不到)

```SH
# 创建
ansible db -m group -a 'name=nginx gid=88 system=yes'

# 删除
ansible db -m group -a 'name=nginx state=absent'
```
#### 3.1.13 Lineinfie模块 ####

ansible在使用sed 进行替换的时候，经常会遇到需要转义的问题，而且ansible在遇到特殊符号进行替换时，存在问题，无法正常替换。

ansible自身模块（两种）

+ lineinfile 模块
+ replace 模块

可以很方便的进行替换，功能：`相当于sed，可以修改文件内容`

```SH
# 修改SELINUX
ansible db -m lineinfile -a "path=/etc/selinux/config regexp='^SELINUX=' line=SELINUX=enforcing"
# 删除注释行
ansible db -m lineinfile -a 'dest=/etc/fstab state=absent regexp="^#"'
```

#### 3.4.16 replace模块 ####

该模块优点类似于 sed命令。主要也是基于正则表达式匹配和替换

示例：

```sh
ansible db -m replace -a "path=/etc/fstab regexp='^(UUID.*)' replace='#\1'"
```

#### 3.4.17 Setup 模块 ####

功能：setup 模块来`收集主机的系统信息`，这些facts信息可以直接以变量的形式使用，但是如果主机较多，会影响执行速度，

使用：`gather_facts:no` 来禁止ansible收集facts信息

+ filter : 过滤器
+ gather

```SH
ansible all -m setup -a 'gather_subset=!all'
ansible all -m setup -a 'gather_subset=!any'
ansible db -m setup -a 'filter=ansible_user_shell'
ansible db -m setup -a 'filter=ansible_hostname'
ansible db -m setup -a 'filter=ansible_distribution_version'
ansible db -m setup -a 'filter=ansible_distribution'
ansible db -m setup -a 'filter=ansible_all_ipv4_addresses'
```

用途：作为条件判断（系统、版本、hostname、CPU、内存、IP等）