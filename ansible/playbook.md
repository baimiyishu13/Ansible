### Playbook ###
---
[Last modification date：2000/10/24]

#### 1.1 playbook介绍 ####

 ![ansible-playbook](../images/image-20221024072909766.png)
 Playbook：

+ 一个或多个 Play组成
+ 功能：预定义的一组主机，装扮成事先通过ansible中的task定义好的角色，Task调用ansible的一个module
+ YAML语言编写

#### 1.2 YAML语言 ####

可读性非常高，k8s也使用yaml语言

YAML：标记语言

**（1）语法简介：**

+ `单一文件`:选择性  三个`-`开始；`...`结束 （可以在一个文件写多个yaml）
+ 缩进统一、区别大小写
+ key/value：同行或分行
+ value：字符串、布尔等
+ 一个完整的代码块必须包含：name、task
+ 一个name只能包含一个task
+ 拓展名：yaml、yml

**（2）List列表：**

列表：由多个元素组成，且所有元素前以 `-` 打头

如：

```yaml
- name: Install a list of packages
  yum:
    name:
      - nginx
      - postgresql
      - postgresql-server  # 也可写为 [nginx,postgresql,postgresql-server]
    state: present
```

**（3）Dictionary 字典**

通常由多个key和value构成，注意格式

```yaml
name: tom
job: it
skill: c++
# 可以放置在{ }中进行表示，用，分割多个key:value
{name: "tom",job: "it",skill: "c++"}
```

列表中可嵌入字典



#### 1.3 Playbook 核心元素 ####

+ Hosts：被控节点列表
+ Tasks：任务集
+ Variables：内置变量或自定义变量在playbook调用
+ Templates：模板，可替换模板文件中的变量，并实现一些简单逻辑的文件
+ Handlers 和 notfiy 结合使用，由特定条件触发（满足、不满足）
+ tags 标签指定某条任务的执行，选择运行部分代码。自动跳过没有变化的部分。

##### 1.3.1 host 组件 #####

hosts：playbook目的让指定的被控节点执行任务，hosts指定被控节点有哪些。

```YAML
- hosts: db
```

##### 1.3.2 remote_user #####

指定远程执行任务的用户（可不写）

+ 可以配合sudo切换用户

```yaml
- hosts: db
  remote_user: root
  tasks:
    - name: test
      ping:
      remote_user: xx
      sudo: yes		# 默认 root
      sudo_user: yy
```

##### 1.3.3 task列表和action组件 #####

调用模块：两种语法

1. action：模块名 参数
2. module：参数 `推荐方法`

注意：shell和command模块后跟命令

```YAML
---
- hosts: db
  remote_user: root
  tasks: 
    - name: install httpd
      yum: 
        name: httpd
        state: present
    - name: start httpd
      service:
        name: httpd
        state: started
        enabled: yes
```

##### 1.3.4 其他组件 #####

某任务的状态运行后为changed时，可以通过`notify`通知给相应的handlers

任务可以通过`tags`打标签，可在ansible-playbook命令上使用 -t 指定进行调用



##### 1.3.5 shell脚本 vs playbook #####

shell脚本

```SH
#!/bin/sh
yum install httpd -y &>/dev/null
cp /tmp/httpd.conf /etc/httpd/conf/httpd.conf
cp /tmp/vhosts.conf /etc/httpd/conf.d/
systemctl enable --now httpd
```

playbook实现

```yaml
---
- hosts: db
  remote_user: root
  tasks: 
    - name: install httpd
      yum: 
        name: httpd
        state: present
    - name: copy file1
      copy: 
        src: /tmp/httpd.conf
        dest: /etc/httpd/conf/httpd.conf
    - name: copy file2
      copy: 
        src: /tmp/vhosts.conf 
        dest: /etc/httpd/conf.d/
    - name: start httpd
      service:
        name: httpd
        state: started
        enabled: yes
```

无需手动将文件复制到远程

### 2.2 playbook 命令 ###

格式：

ansible-playbook <filename> [options]

常见选项

+ `-C（大C）`： - -check 只检测可能发生的改变，`不真正执行`
+ `--list-hosts`：列出被控节点
+ `--list-tags`：列出tag
+ `--limit db`：只针对dn组的被控节点执行
+ `-v -vv -vvv`：详细信息

