### Linux下安装Python3.11.0

---

1.安装依赖环境

```SH
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
```

2.官方下载Python11.0

官方：https://www.python.org/downloads/release/python-3110/

最下边下载`[Gzipped source tarball](https://www.python.org/ftp/python/3.11.0/Python-3.11.0.tgz)`

直接在linux下载：

```SH
wget https://www.python.org/ftp/python/3.11.0/Python-3.11.0.tgz
```

3.根据个人喜好选择Python安装位置

```SH
mkdir -p /usr/local/python3.11
```

4.解压：

```SH
mv Python-3.11.0.tgz /usr/local/python3.11/
cd  /usr/local/python3.11/ 
tar -zxvf Python-3.11.0.tgz
```

5.进入解压后的目录，编译安装

```SH
cd Python-3.11.0
./configure --prefix=/usr/local/python3.11
```

注意：

+ `–-prefix` 参数是**指定安装目录**，就是上边第一步创建的目录
+ 如果在这里没有指定安装目录：需要到`/usr/local/bin`或`/usr/local/lib`目录下

6.编译 make

7.编译成功后，编译安装：make install

注意：可写成`make && make install`

8.环境变量配置

`vi /etc/profile`

在最下边输入

```SH
export PYTHON_HOME=/usr/local/python3.11
export PATH=${PYTHON_HOME}/bin:$PATH
```

9.使得配置的环境变量立即生效：

```SH
source /etc/profile
```

---

注意：如果是上边安装目录没有指定，就不需要配置环境变量。因为Python3.9默认安装的目录`/usr/local/bin`是在环境变量中的。

```SH
[root@matser ~]# python3.11
Python 3.11.0 (main, Oct 27 2022, 10:52:31) [GCC 4.8.5 20150623 (Red Hat 4.8.5-44)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

ln -s python3 /usr/bin/python3

