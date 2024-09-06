## 	Linux入门考核

# 0.计算机基础

## 0.1名词解释

- CPU

- 内存

- 虚拟内存

- 存储

- 环境变量

- 二进制

- 根用户

- IP

- Host0526

- 套接字（socket）:ip+port

- 端口（port）:0~65535

- 相对路径

- 绝对路径

## 0.2 常用命令

- ping
- grep
- reboot
- |
- curl

## 0.3 常用快捷键

- tab：自动补齐命令或文件
- ctrl+c：强制终止进程（暴力结束）
- ctrl+d：退出当前命令行
- shift+pageUp：向上翻页
- shift+pageDown：向下翻页

## 0.4 高危操作命令

- rm -rf /
- chmod -R 777 /
- \> 文件

## 0.5 扩展命令

-  bc 计算器：需要显示小数点后的结果，需输入scale=number命令，number代表小数点后的位数

## 0.6 nano文本编辑器

**基本语法**

nano	文件名称

**选项说明**

| 选项   | 说明                                         |
| ------ | -------------------------------------------- |
| ctrl+G | 获取帮助文档                                 |
| ctrl+X | 离开nano软件，如有修改文件会提示是否需要保存 |
| ctrl+O | 保存文件，需有文件写入权限                   |
| ctrl+R | 从其他文件读入文本                           |
| ctrl+C | 说明光标所在行和列的信息                     |
| ctrl+K | 剪切文本，可以一次剪切多行                   |
| ctrl+U | 粘贴文本                                     |
| ctrl+W | 查找字符串                                   |
| ctrl+Y | 上一页                                       |
| ctrl+V | 下一页                                       |

## 0.7 查看操作系统相关信息

- 查看内核版本

  ```shell
  [root@localhost ~]# uname -r
  3.10.0-1160.el7.x86_64
  ```

- 查看系统架构版本

  ```shell
  [root@localhost ~]# uname -m
  x86_64
  ```

- 查看CPU型号
  
  ```shell
  [root@k8s-master kubesphere]# lscpu 
  架构：                           aarch64
  CPU 运行模式：                   64-bit
  字节序：                         Little Endian
  CPU:                             16
  在线 CPU 列表：                  0-15
  每个核的线程数：                 1
  每个座的核数：                   16
  座：                             1
  NUMA 节点：                      1
  厂商 ID：                        Phytium
  型号：                           3
  型号名称：                       ARMv8 CPU
  步进：                           0x1
  ```

- 查看系统架构

  ```shell
  [root@k8s-master kubesphere]# arch
  aarch64
  ```


## 0.8 安全相关

- 查看ssh登录历史记录

  ```shell
  [root@k8s-master kubesphere]# last
  ```

## 0.9 journalct 查看日志

- 查看实时系统日志

  ```shell
  [root@k8s-master kubesphere]# journalctl -f
  ```

- 查看指定服务日志

  ```shell
  [root@k8s-master kubesphere]# journalctl -u ss
  ```

## 0.10 查看操作系统版本

- nkvers

# 1.基础部分

## 1.1用户管理命令

### 1.1.0相关介绍

#### 1-SSH远程登录

**基本语法**

ssh IP地址

**案例实操**

```ssh
ssh 192.168.91.128
```

#### 2-SSH注销登录

**基本语法**

logout	（功能描述：仅限通过ssh登录使用）

**案例实操**

```ssh
[root@localhost ~]# logout
```

### 1.1.1基本命令

#### **1-添加用户（useradd）** 

**基本语法**

**添加新用户** 

useradd 用户名 

**案例实操**

```shell
[root@localhost ~]# useradd tony
```

**添加新用户到某个组**

useradd -g 用户组名 用户名

**案例实操**

```shell
[root@localhost ~]# useradd -g java jack
```

#### **2-设置用户密码（passwd）**

**基本语法**

passwd 用户名

**案例实操**

```shell
[root@localhost ~]# passwd tony
```

#### **3-查看用户属性（id）**

**基本语法**

id 用户名

**案例实操**

```shell
[root@localhost ~]# id tony
uid=1002(tony) gid=1003(tony) 组=1003(tony)
#也可用来判断用户是否存在
[root@localhost ~]# id maria
id: maria: no such user
```

#### **4-/etc/passwd 查看创建的所有用户**

**基本语法**

cat /etc/passwd

**案例实操**

```shell
[root@localhost ~]# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
#省略中间部分创建的系统用户
#省略中间部分创建的系统用户
#省略中间部分创建的系统用户
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
tcpdump:x:72:72::/:/sbin/nologin
admin:x:1000:1000:cent0S7.9:/home/admin:/bin/bash
tom:x:1001:1002::/home/tangmu:/bin/bash
tony:x:1002:1003::/home/tony:/bin/bash
jack:x:1003:1004::/home/jack:/bin/bash
```

#### **5-su 切换用户**

**基本语法**

**切换用户（不获得环境变量）**

su 用户名称

**案例实操**

```shell
[root@localhost ~]# su tony
[tony@localhost root]$ $PATH
bash: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin: 没有那个文件或目录
```

**切换用户（获得环境变量）**

su - 用户名称

**案例实操**

```shell
[root@localhost ~]# su - tony
上一次登录：四 1月 26 17:07:15 CST 2023pts/0 上
[tony@localhost ~]$ $PATH
-bash: /usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/tony/.local/bin:/home/tony/bin: 没有那个文件或目录
```

**返回切换前的用户**

exit

**案例实操**

```shell
[tony@localhost ~]$ exit
登出
[root@localhost ~]# 
```

#### **6-usermod 修改用户所属组**

**基本语法**

usermod -g 用户组名 用户名

**案例实操**

```shell
[root@localhost home]# usermod -g java ken
```

#### **7-userdel 删除用户**

**基本语法**

**删除用户（删除用户，但保留用户主目录）**

userdel 用户名

**案例实操**

```shell
[root@localhost home]# userdel tony
#home文件夹下仍保留tony的主目录
[root@localhost home]# ls
admin  jack  my-abc  my-info  tangmu  tony
```

**删除用户（删除用户，不保留用户主目录）**

userdel -r 用户名

**案例实操**

```shell
[root@localhost home]# userdel -r jack
#连同jack的主目录一并删除
[root@localhost home]# ls
admin  my-abc  my-info  tangmu  tony
```

#### **8-who 查看登录用户信息**

查看实际登录用户信息（显示登录IP和登录时间）

**基本语法**

who 或 who am i

**案例实操**

```shell
[ken@localhost home]$ who am i
root     pts/0        2023-01-26 16:42 (192.168.91.1)
[ken@localhost home]$ who
root     pts/0        2023-01-26 16:42 (192.168.91.1)
```

**查看当前使用用户信息（su之后）**

whoami

**案例实操**

```shell
[ken@localhost home]$ whoami
ken
```

#### **9-sudo 普通用户获取root权限**

**基本语法**

sudo	[选项]	需要执行的命令

**选项说明**

| 选项 | 说明                         |
| ---- | ---------------------------- |
| -l   | 显示当前用户可执行的sudo命令 |

**案例实操**

```shell
[ken@localhost home]$ sudo passwd ken
```

### 1.1.2 考核内容

1. 创建一个以自己名字命名的账户

2. 为新建立的账户设置密码

3. 使用root用户切换至新用户

4. 切换回root用户

5. 删除新用户，并保留用户主目录

6. 判断删除的新用户是否还存在

7. 查看主机上的所有用户

## 1.2 用户组管理命令

一个用户可以拥有多个用户组

   ### 1.2.1 基本命令

#### **1-groupadd 新增用户组**

**基本语法**

groupadd 组名

**案例实操**

```shell
[root@cent0S7 ~]# groupadd web
```

#### **2-groupdel 删除用户组**

**基本语法**

groupdel 组名

**案例实操**

```shell
[root@cent0S7 ~]# groupdel web
```

#### **3-groupmod 修改用户组**

**基本语法**

groupmod -n 新用户组名  旧用户组名

**案例实操**

```shell
[root@cent0S7 home]# groupmod -n ad admin
```

#### **4-/etc/group 查询所有用户组**

**基本语法**

cat /etc/group

**案例实操**

```shell
[root@cent0S7 home]# cat /etc/group
root:x:0:
#省略中间部分创建的系统用户
#省略中间部分创建的系统用户
#省略中间部分创建的系统用户
docker:x:981:
tony:x:1001:
ad:x:1000:admin
```

### 1.2.2 考核内容

1. 创建java用户组
2. 查看所有用户组
3. 修改java用户组为python用户组
4. 删除python用户组
5. 将自己名字的用户加入到python组（详见1.1用户管理命令usermod）

## 1.3时间日期命令

### 1.3.1基本命令

#### **1-date 显示当前时间**

**基本语法**

date

**案例实操**

- 显示当前时间

```shell
[root@cent0S7 home]# date
2023年 02月 02日 星期四 16:42:04 CST
```

- date +%Y 显示当前年份（year）

```sehll
[root@cent0S7 ~]# date +%Y
  2023
```

- date +%m 显示当前月份（month）

```shell
  [root@cent0S7 ~]# date +%m
  02
```

- date +%d 显示当月天数（days）

```SHELL
  [root@cent0S7 ~]# date +%d
  03
```

- date +%H 显示当前小时（hours）

```shell
  [root@cent0S7 ~]# date +%H
  14
```

- date +%M 显示当前分钟（minutes）

```shell
  [root@cent0S7 ~]# date +%M
  42
```

- date +%S 显示当前秒数（seconds）

```SHELL
  [root@cent0S7 ~]# date +%S
  37
```

#### **2-date  修改当前时间**

**基本语法**

date -s 需要修改的时间字符串

**案例实操**

```shell
[root@cent0S7 ~]# date -s "2023-02-03 14:32:00"
2023年 02月 03日 星期五 14:32:00 CST
```

#### **3-cal 查看日历**

**基本语法**

cal	[month]	[year]

**案例实操**

显示当月日历

cal

```shell
[root@cent0S7 ~]# cal
      二月 2023     
 日 一 二  三 四  五 六
          1  2  3  4
 5  6  7  8  9 10 11
12 13 14 15 16 17 18
19 20 21 22 23 24 25
26 27 28

```

 显示指定年份日历

cal 查看的年份

```shell
[root@cent0S7 ~]# cal 2023
                               2023                               

        一月                   二月                   三月        
 日 一 二  三 四 五 六    日 一 二 三 四 五 六       日 一 二 三  四 五 六
 1  2  3  4  5  6  7             1  2  3  4             1  2  3  4
 8  9 10 11 12 13 14    5  6  7  8  9 10 11    5  6  7  8  9 10 11
15 16 17 18 19 20 21   12 13 14 15 16 17 18   12 13 14 15 16 17 18
22 23 24 25 26 27 28   19 20 21 22 23 24 25   19 20 21 22 23 24 25
29 30 31               26 27 28               26 27 28 29 30 31
####后续月份省略
####后续月份省略
####后续月份省略
```

同时指定年份和月份

```shell
[root@cent0S7 ~]# cal 2 2023
      二月 2023     
日 一 二 三 四 五 六
          1  2  3  4
 5  6  7  8  9 10 11
12 13 14 15 16 17 18
19 20 21 22 23 24 25
26 27 28
```

#### **4-timedatectl 设置时区**

**基本语法**

timedatectl	[选项]	（功能描述：管理时区相关信息）

**选项说明**

| 选项                      | 说明                           |
| ------------------------- | ------------------------------ |
| status                    | 显示当前时间、日期、时区等信息 |
| list-timezones            | 看所有可用的时区               |
| set-timezone <设置的时区> | 设置本机的时区                 |

**案例实操**

```shell
#显示当前时间、日期、时区等信息
[root@localhost ~]# timedatectl status
      Local time: Fri 2023-04-07 14:50:59 CST
  Universal time: Fri 2023-04-07 06:50:59 UTC
        RTC time: Fri 2023-04-07 06:50:53
       Time zone: Asia/Shanghai (CST, +0800)
     NTP enabled: yes
NTP synchronized: no
 RTC in local TZ: no
      DST active: n/a
#查看所有可用的时区
[root@localhost ~]#  timedatectl list-timezones
Africa/Abidjan
Africa/Accra
Africa/Addis_Ababa
Africa/Algiers
Africa/Asmara
...............
#设置系统的时区为上海
[root@localhost ~]# timedatectl set-timezone Asia/Shanghai
```

### 1.3.2考核内容

1. 显示当前时间
2. 修改当前时间
3. 显示当月日历

 ## 1.4文件目录类命令

### 1.4.0 相关介绍

**Linux中，一切皆文件。**

根目录结构如下所示：

| 目录名称          | 用途                                                         |
| ----------------- | ------------------------------------------------------------ |
| 🚩/bin             | Binary的缩写，存放可执行的命令二进制文件                     |
| 🚩/sbin            | super User，存放超级管理员可执行的命令                       |
| 🚩/home            | 普通用户的主目录，在Linux中每个用户都有一个自己的目录，一般是以账号的名称命名目录 |
| 🚩/root            | 系统管理员（超管）的主目录                                   |
| /lib              | 系统开机所需要的动态链接共享库，作用类似于Windows里的DLL文件，大部分应用程序都会用到此共享库 |
| /lost+found       | 这个目录一般情况下是空的，当系统非法关机后，这里会存放一些文件 |
| /run              | 是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除 |
| 🚩/etc             | 存放所有的系统管理所需要的配置文件和子目录                   |
| 🚩/usr             | 用户的应用程序和文件都存放在这个目录                         |
| /boot             | 存放linux启动时的一些核心文件                                |
| /proc（禁止操作） | 这个目录是虚拟目录，是系统内存的映射，可以直接访问获取系统信息 |
| /srv（禁止操作）  | service的缩写，存放一些服务启动后需要提前的数据              |
| /sys（禁止操作）  | linux2.6内核新出现的内核sysfs                                |
| 🚩/tmp             | 用于存放一些临时文件                                         |
| /dev              | 类似于windows的设备管理器，把所有的硬件通过文件的形式存储    |
| /media            | centOS6:linux系统会自动识别一些设备，例如：U盘，光驱等，并挂载到这个目录下<br/>centOS7:迁移到/run/media |
| 🚩/mnt             | 系统提供目录，让用户临时挂载别的文件系统，可以将外部存储挂载到此目录 |
| 🚩/opt             | 给主机额外安装软件的目录，建议软件安装包存放在此目录下，如：mysql |
| 🚩/var             | 这个目录用于存放不断扩充的东西，如：各种日志文件             |



### 1.4.1基本命令

#### **1-pwd 查看当前目录**

pwd：print working directory 

**基本语法**

**显示当前目录的绝对路径**

pwd

**案例实操**

```shell
[root@cent0S7 ~]# pwd
/root
```

#### **2-ls 列出目录下文件内容**

ls：list 

**基本语法**

ls 	\[选项]	[目录或文件]

| 选项        | 功能                                                         |
| ----------- | ------------------------------------------------------------ |
| -a          | 显示全部的文件，包括隐藏的文件                               |
| -l          | 显示文件的完整信息，包括的属性、权限等数据（等价于命令“ll”） |
| -h          | 以更加便于阅读的格式输出文件大小（如 1K 234M  2G）           |
| --full-time | 显示更为详细的时间信息                                       |

**案例实操**

```shell
#显示全部文件
[root@cent0S7 ~]# ls -a
.                .bashrc    .ICEauthority         study        视频
..               .cache     initial-setup-ks.cfg  .tcshrc      图片
anaconda-ks.cfg  .config    .local                .viminfo     文档
.bash_history    .cshrc     mariadb:10.3.12.tar   .Xauthority  下载
.bash_logout     .dbus      mariadb.tar           公共         音乐
.bash_profile    .esd_auth  .pki                  模板         桌面
#显示文件的完整信息
[root@cent0S7 ~]# ls -l
总用量 771880
#文件类型与权限 链接数 文件属主 文件属组 文件大小（byte表示） 创建或最后修改的时间 文件或目录名字 
-rw-------. 1 root root      1887 12月  7 16:24 anaconda-ks.cfg
-rw-r--r--. 1 root root      1935 12月  7 22:56 initial-setup-ks.cfg
-rw-------. 1 root root 374157312 12月 15 16:12 mariadb:10.3.12.tar
-rw-------. 1 root root 416237056 12月 15 14:52 mariadb.tar
drwxr-xr-x. 2 root root        30 1月  10 15:39 study
drwxr-xr-x. 2 root root         6 12月  8 09:40 公共
drwxr-xr-x. 2 root root         6 12月  8 09:40 模板
drwxr-xr-x. 2 root root         6 12月  8 09:40 视频
drwxr-xr-x. 2 root root         6 12月  8 09:40 图片
drwxr-xr-x. 2 root root         6 12月  8 09:40 文档
drwxr-xr-x. 2 root root         6 12月  8 09:40 下载
drwxr-xr-x. 2 root root         6 12月  8 09:40 音乐
drwxr-xr-x. 2 root root         6 12月  8 09:40 桌面
#显示文件的完整信息，并以便于阅读的格式展示文件大小
[root@cent0S7 ~]# ls -lh
总用量 754M
-rw-------. 1 root root 1.9K 12月  7 16:24 anaconda-ks.cfg
-rw-r--r--. 1 root root 1.9K 12月  7 22:56 initial-setup-ks.cfg
-rw-------. 1 root root 357M 12月 15 16:12 mariadb:10.3.12.tar
-rw-------. 1 root root 397M 12月 15 14:52 mariadb.tar
drwxr-xr-x. 2 root root   30 1月  10 15:39 study
drwxr-xr-x. 2 root root    6 12月  8 09:40 公共
drwxr-xr-x. 2 root root    6 12月  8 09:40 模板
drwxr-xr-x. 2 root root    6 12月  8 09:40 视频
drwxr-xr-x. 2 root root    6 12月  8 09:40 图片
drwxr-xr-x. 2 root root    6 12月  8 09:40 文档
drwxr-xr-x. 2 root root    6 12月  8 09:40 下载
drwxr-xr-x. 2 root root    6 12月  8 09:40 音乐
drwxr-xr-x. 2 root root    6 12月  8 09:40 桌面
```

#### **3-cd 切换目录**

cd：change directory

**基本语法**

cd [参数]

| 参数        | 功能                             |
| ----------- | -------------------------------- |
| cd 相对路径 | 切换路径                         |
| cd 绝对路径 | 切换路径                         |
| cd ~ 或 cd  | 回到自己的home主目录             |
| cd -        | 切换至上一次访问的目录           |
| cd ..       | 回到当前目录上一级目录           |
| cd -P       | 跳转到实际物理路径，而非快捷方式 |

**案例实操**

```shell
#使用相对路径切换目录
[root@cent0S7 home]# pwd
/home
[root@cent0S7 home]# ls
admin  tony
[root@cent0S7 home]# cd admin/
#使用绝对路径切换目录
[root@cent0S7 admin]# cd /home/tony/
#回到自己的home主目录
[root@cent0S7 tony]# cd
[root@cent0S7 ~]# pwd
/root
#回到上一级目录
[root@cent0S7 ~]# cd ../
[root@cent0S7 /]# pwd
/
#回到上一次访问的目录
[root@cent0S7 /]# cd -
/root
[root@cent0S7 ~]# cd -
/
#跳转实际物理路径
[root@cent0S7 /]# pwd
/
[root@cent0S7 /]# cd -P bin/
[root@cent0S7 bin]# pwd
/usr/bin
```

#### **4-mkdir 创建新目录**

mkdir：make directory

**基本语法**

mkdir 	[选项]	要创建的目录

| 选项 | 功能         |
| ---- | ------------ |
| -p   | 创建多层目录 |

**案例实操**

```shell
#创建一层目录
[root@cent0S7 tony]# mkdir gitlab
#创建多层目录
[root@cent0S7 tony]# mkdir -p workspaces/java
```

#### **5-rmdir 删除空目录**

rmdir：remove directory

**基本语法**

rmdir 需要删除的空目录

**案例实操**

```shell
[root@cent0S7 tony]# rmdir gitlab/
```

#### **6-touch 创建空文件**

**基本语法**

touch 文件名称

**案例实操**

```shell
[root@cent0S7 ~]# touch abc.txt
```

#### **7-cp 复制文件或目录**

**基本语法**

cp	[选项]	源文件 目标文件

| 选项 | 功能               |
| ---- | ------------------ |
| -r   | 递归复制整个文件夹 |

**案例实操**

```shell
#复制单个文件
[root@cent0S7 ~]# cp abc.txt /home/tony/
#递归复制整个文件夹
[root@cent0S7 ~]# cp -r /home/tony/ /home/admin/
```

#### **8-rm 删除文件或目录**

**基本语法**

rm	[选项] 目标文件

| 选项 | 功能                                 |
| ---- | ------------------------------------ |
| -r   | 递归删除目录中所有内容               |
| -f   | 强制执行删除操作，不提示确认删除选项 |
| -v   | 显示指令执行的详细过程               |

**案例实操**

```shell
#删除单个文件
[root@cent0S7 admin]# rm hello
rm：是否删除普通文件 "hello"？y
#递归强制删除文件夹
[root@cent0S7 admin]# rm -rf tony/
```

#### **9-mv 移动文件、目录或重命名**

**基本语法**

mv 源文件 目标文件 （功能描述：同一目录下使用此命令为重命名，不同目录下为移动文件且同时可指定新文件名）

**案例实操**

```shell
#重命名文件
[root@cent0S7 admin]# mv hello hello2
#移动文件夹并进行重命名
[root@cent0S7 admin]# mv /home/tony/ /home/admin/tony/
```

#### **10-cat 查看文件全部内容**

查看文件全部内容，从第一行开始

**经验技巧**

仅适用于查看小文件，一屏幕就可完整显示的情况

**基本语法**

cat 	[选项]	查看的文件

| 选项 | 功能     |
| ---- | -------- |
| -n   | 显示行号 |

**案例实操**

```shell
#查看文件内容
[root@cent0S7 admin]# cat hello 
abc
def
ghi
#查看文件内容并显示行号
[root@cent0S7 admin]# cat -n hello 
     1	abc
     2	def
     3	ghi
```

#### **11-more 文件内容分屏显示**

基于vi 编辑器，一次性加载全部文件，可以进行分屏展示

**基本语法**

more	[选项] 	查看的文件

**选项说明**

| 选项   | 说明            |
| ------ | --------------- |
| +行号X | 从第X行开始显示 |
| -行号X | 一屏幕只显示X行 |

**操作说明**

| 操作            | 说明                       |
| --------------- | -------------------------- |
| 空格键（space） | 向下滚动一屏               |
| 回车键（enter） | 向下滚动一行               |
| q               | 退出more，不再显示文件内容 |
| ctrl+f          | 向下滚动一屏               |
| ctrl+b          | 向上滚动一屏               |
| =               | 输出当前行号               |
| :f              | 输出当前文件名和房号       |
| v               | 调用vi编辑器               |

**案例实操**

```shell
#从第500行开始显示
[root@cent0S7 admin]# more +500 三国演义.txt 
```

#### **12-less 文件内容分屏显示**

less与more类似，具有字符串查找功能，并不是一次性加载整个文件，而是根据需要进行加载，==对显示大型文件具有较高的效率==。

**基本语法**

less	[选项]	查看的文件

**选项说明**

| 选项   | 说明            |
| ------ | --------------- |
| +行号X | 从第X行开始显示 |
| -行号X | 一屏幕只显示X行 |

**操作说明**

| 操作               | 说明                                                 |
| ------------------ | ---------------------------------------------------- |
| 空格键（space）    | 向下滚动一屏                                         |
| 回车键（enter）    | 向下滚动一行                                         |
| q                  | 退出less，不再显示文件内容                           |
| ctrl+f 或 pagedown | 向下滚动一屏                                         |
| ctrl+b 或 pageup   | 向上滚动一屏                                         |
| /字符串            | 向下查找字符串；n：向下查找；N：向上查找；           |
| ?字符串            | 向上查找字符串；n：向上查找；N：向下查找；           |
| =                  | 显示当前行号、字节数、当前位置占全文内容百分比等信息 |

**案例实操**

```shell
#从第500行开始显示
[root@cent0S7 admin]# less +500 三国演义.txt
```

#### **13-echo 输出内容至控制台**

**基本语法**

echo	[选项]	输出内容

**选项说明**

| 选项 | 说明                           |
| ---- | ------------------------------ |
| -e   | 允许对反斜线转义的字符进行解释 |

**转义字符**

| 转义字符 | 说明      |
| -------- | --------- |
| \\       | 输出\本身 |
| \n       | 换行符    |
| \t       | 制表符    |

**案例实操**

```shell
#输出一行内容
[root@cent0S7 admin]# echo hello
hello
#输出制表符
[root@cent0S7 admin]# echo -e hello\\tworld
hello	world
#输出换行符
[root@cent0S7 admin]# echo -e hello\\nworld
hello
world
```

#### **14-haed 显示文件头部内容**

用于输出文件头部的内容，默认情况下显示文件前10行

**基本语法**

head	[选项]	文件名称

**选项说明**

| 选项      | 说明                   |
| --------- | ---------------------- |
| -n <行数> | 指定显示头部内容的行数 |

**案例实操**

```shell
#显示文件前20行内容
[root@cent0S7 admin]# head -20 三国演义.txt 
```

#### **15-tail 显示文件尾部内容**

用于输出文件末尾的内容，默认情况下显示文件后10行

**基本语法**

tail	[选项]	文件名称

**选项说明**

| 选项      | 说明                                 |
| --------- | ------------------------------------ |
| -n <行数> | 指定显示尾部内容的行数               |
| -f        | 显示文件最新追加的内容，监视文件变化 |

**案例实操**

```shell
#显示文件后5行的内容
[root@cent0S7 ~]# tail -n 5 initial-setup-ks.cfg 
%anaconda
pwpolicy root --minlen=6 --minquality=1 --notstrict --nochanges --notempty
pwpolicy user --minlen=6 --minquality=1 --notstrict --nochanges --emptyok
pwpolicy luks --minlen=6 --minquality=1 --notstrict --nochanges --notempty
%end
#实时显示文件最新追加的内容
[root@cent0S7 ~]# tail -f initial-setup-ks.cfg 
```

#### **16-\> 输出重定向和 >> 追加写**

\> 表示输出重定向，即覆盖文件原本内容

\>> 表示追加写，即往文件末尾进行追加写

**案例实操**

```shell
#将ls展示的信息输出至ls.info文件，并覆盖原本内容
[root@cent0S7 ~]# ls -l >> ls.info
[root@cent0S7 ~]# cat ls.info 
总用量 771880
-rw-r--r--. 1 root root         0 2月   7 08:46 abc.txt
-rw-------. 1 root root      1887 12月  7 16:24 anaconda-ks.cfg
-rw-r--r--. 1 root root      1935 12月  7 22:56 initial-setup-ks.cfg
#往echo.info追加写入内容
[root@cent0S7 ~]# cat echo.info 
hello
[root@cent0S7 ~]# echo world >> echo.info 
[root@cent0S7 ~]# cat echo.info 
hello
world
```

#### **17-ln 软链接硬链接**

**软链接**

- 类似于windows系统中的快捷方式，以一个链接的形式指向源文件
- 可以对不存在的文件名进行链接
- 可以对目录进行链接
- 可以跨文件系统
- 源文件删除后将无法访问

**基本语法**

ln -s 源目标 链接名

**案例实操**

```shell
#将文件f1创建一个软链接f2
[root@cent0S7 admin]# ln -s f1 f2
```

**经验技巧**

==删除软链接时，应使用 rm -r 软连接名 ，禁止使用 rm -rf 软链接名/（此命令会删除真实目录下全部内容）==

**硬链接**

- 以文件副本的形式存在，但不占用实际空间
- 只能对存在的文件进行链接
- 不能对目录进行链接
- 不可以跨文件系统
- 源文件删除后，仍可以进行访问

**基本语法**

ln  源目标 链接名

**案例实操**

```shell
#将文件f1创建一个硬链接f2
[root@cent0S7 admin]# ln f1 f2
```

**经验技巧**

软链接和硬链接会保持每一处链接文件的同步性，即无论改动了哪一处，所有的文件都会发生相同的变化

#### **18-history 显示已经成功执行的历史命令**

**基本语法**

history	[选项]

**选项说明**

| 选项   | 说明                   |
| ------ | ---------------------- |
| <行数> | 显示最后执行的几行命令 |
| -c     | 清除历史命令           |

**经验技巧**

! <历史命令序号>   可以重复执行历史命令

**案例实操**

```shell
#显示历史命令
[root@cent0S7 admin]# history 
    1  history 
    2  ls
    3  df -h
    4  ll
    5  cd ../
    6  ls
#显示最后3条历史命令
[root@cent0S7 admin]# history 3
    8  ls
    9  history 
   10  history 3
#再次执行第3条命令
[root@cent0S7 admin]# !3
df -h
文件系统        容量  已用  可用 已用% 挂载点
devtmpfs        895M     0  895M    0% /dev
tmpfs           910M     0  910M    0% /dev/shm
tmpfs           910M   11M  900M    2% /run
tmpfs           910M     0  910M    0% /sys/fs/cgroup
/dev/sda3        27G  7.0G   21G   26% /
/dev/sda1      1014M  230M  785M   23% /boot
tmpfs           182M   12K  182M    1% /run/user/42
tmpfs           182M     0  182M    0% /run/user/0
```

### 1.4.2考核内容

1. 查看当前目录绝对路径

2. 显示home主目录下的文件内容

3. 在/opt目录下创建一个自己名字的目录

4. 在刚新创建的目录下新建一个文件abc.txt

5. 将abc.txt更改为aaa.txt

6. 复制aaa.txt为bbb.txt

7. 删除aaa.txt、bbb.txt

8. 删除第3步所创建的目录

9. 阐述cat、more、less、head、tail查看文件的区别

10. 新建一个文件，尝试使用echo往文件里面覆盖写入和追加写入，并实时监听文件内容的变化

11. 尝试建立一个软链接，并说明软链接和硬链接的区别

12. 查看历史命令

## 1.5 VI/VIM编辑器

VI 是 Unix 操作系统和类 Unix 操作系统中最通用的文本编辑器。

VIM 编辑器是从 VI 发展出来的一个性能更强大的文本编辑器。可以主动的以字体颜色辨别语法的正确性，方便程序设计。VIM 与 VI 编辑器完全兼容。

**基本语法**

vi/vim	文件名称

### 1.5.1一般模式

**基本语法**

- 打开文件后，默认进入的就是一般模式，可以移动光标、复制、删除文字
- 在编辑模式和指令模式中按【esc】会进入一般模式

**操作说明**

| 操作         | 说明                                         |
| ------------ | -------------------------------------------- |
| yy           | 复制光标当前一行                             |
| y 数字 y     | 复制一段（光标当前行开始，至后续指定数字行） |
| p            | 从当前光标位置进行粘贴                       |
| dd           | 删除光标当前行                               |
| d 数字 d     | 删除一段（光标当前行开始，至后续指定数字行） |
| yw           | 复制一个词                                   |
| dw           | 删除一个词                                   |
| u            | 撤销上一步                                   |
| x            | 删除光标当前位置字符                         |
| X            | 上传光标前一位置字符                         |
| shift+^      | 移动到行头                                   |
| shift+$      | 移动到行尾                                   |
| shift+g      | 移动到页尾                                   |
| 1+shift+g    | 移动到页头                                   |
| 数字+shift+g | 移动到指定行数                               |

### 1.5.2编辑模式

**基本语法**

- 输入 i、I、o、O、a、A中的任何一个字母都可以进入编辑模式，在画面下方会出现【INSERT】字样，此时可编辑文本内容
- 按 Esc 键后退出编辑模式回到一般模式

**操作说明**

| 操作 | 说明                   |
| ---- | ---------------------- |
| i    | 当前光标前插入         |
| a    | 当前光标后插入         |
| o    | 当前光标行的下一行插入 |
| I    | 光标所在行最前插入     |
| A    | 光标所在行最后插入     |
| O    | 当前光标行的上一行插入 |

### 1.5.3指令模式

**基本语法**

- 在**一般模式** 中，输入【：、/、？】中的任何一个，就可以将光标移动至画面底部进入指令模式
- 按 Esc 键后退出编辑模式回到一般模式

**操作说明**

| 操作          | 说明                                                       |
| ------------- | ---------------------------------------------------------- |
| :w            | 保存                                                       |
| :q            | 退出                                                       |
| :!            | 强制执行（wq!三个操作可组合使用）                          |
| /要查找的词   | n向下查找，N向上查找                                       |
| :noh          | 取消高亮显示                                               |
| :set number   | 显示行号                                                   |
| :set nonumber | 关闭行号                                                   |
| :%s/old/new/g | 替换内容，/g表示替换匹配到的所有内容，不加只会执行一次替换 |

### 1.5.4考核内容

使用VIM编辑器执行以下操作：

- 复制多行
- 删除多行
- 显示行号
- 查找字符
- 替换字符
- 撤销上一步操作
- 使用编辑模式写入任意内容，并保存文件

## 1.6文件权限管理命令

 ### 1.6.1权限属性

Linux系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。

为了保护系统的安全性，Linux系统对不同的用户访问同一文件（包括目录文件）的权限做了不同的规定。在Linux中我们可以使用ll或者ls-l命令来显示一个文件的属性以及文件所属的用户和组。

**案例实操**

```shell
[root@cent0S7 ~]# ll
###中间部分省略
###中间部分省略
###中间部分省略
#文件权限	 #链接数 	#文件属主 #文件属组	  #文件大小 #创建或最近修改时间 #文件名
drwxr-xr-x. 2 		root 	root        30 		1月  10 15:39 	study
drwxr-xr-x. 2 		root 	root         6 		12月  8 09:40 	公共
drwxr-xr-x. 2 		root 	root         6 		12月  8 09:40 	模板
```

文件权限从左至右10位字符具体含义如下：

|          | 文件类型   | 属主权限     | 属组权限     | 其他用户权限 |
| -------- | ---------- | ------------ | ------------ | ------------ |
| **位置** | 0          | 123          | 456          | 789          |
| **示例** | d          | rwx          | r-x          | r-x          |
| **解释** | 目录或文件 | 读、写、执行 | 读、写、执行 | 读、写、执行 |

- 0位：表示文件类型，d 为目录、- 为文件、l 为链接文档、b为可存储设备、c为串行端口设备（键盘、鼠标等）
- 1-3位：表示文件属主的文件权限（user）
- 4-6位：表示属组的文件权限（group）
- 7-9位：表示其他用户拥有的文件权限（ohter）

**rwx作用于文件和目录的不同解释**

1. 作用于文件

   - 【r】可以读取、查看
   - 【w】可以修改，但不一定能删除，删除需要对当前文件夹有写权限
   - 【x】可以被系统执行
   - 【-】表示无权限

2. 作用于目录

   - 【r】可以查看目录内容
   - 【w】可以创建、删除、重命名目录
   - 【x】可以进入目录
   - 【-】表示无权限

### 1.6.2基本命令

#### 1-chmod 改变文件目录权限

   **基本语法**

- 方法一：chmod	[{ugoa}{+-=}{rwx}]	文件或目录

  u：所有者 	g：所属组 	o：其他人 	a：所有人(u、g、o的总和)

  +：新增权限	-：移除权限	=：重新设置权限

  **案例实操**

  ```shell
  #所有者新增执行权限
  [root@cent0S7 master]# chmod u+x abc.txt 
  #所有者移除执行权限
  [root@cent0S7 master]# chmod u-x abc.txt
  #所有者仅设置执行权限（所有者的其他权限会被移除）
  [root@cent0S7 master]# chmod u=x abc.txt 
  #所有人新增读权限
  [root@cent0S7 master]# chmod a+r abc.txt 
  ```

- 方法二：chmod    [mode=421] 文件或目录

​		r=4 w=2 x=1 rwx=4+2+1=7

**案例实操**

```shell
#为所有者设置读写执行权限、为所属组设置读写权限、其他用户不设置任何权限
[root@cent0S7 master]# chmod 760 abc.txt 
[root@cent0S7 master]# ll
总用量 0
-rwxrw----. 1 root root 0 2月  28 10:20 abc.txt
```

**选项说明**

| 选项 | 说明     |
| ---- | -------- |
| -R   | 递归操作 |

#### 2-chown 改变文件目录所有者

**基本语法**

chown	[选项]	[最终用户]	[文件或目录]	

**选项说明**

| 选项 | 说明     |
| ---- | -------- |
| -R   | 递归操作 |

**案例实操**

```shell
#改变文件所有者
[root@cent0S7 master]# ll
总用量 0
-rwxrw----. 1 root root 0 2月  28 10:20 abc.txt
[root@cent0S7 master]# chown admin abc.txt 
[root@cent0S7 master]# ll
总用量 0
-rwxrw----. 1 admin root 0 2月  28 10:20 abc.txt
#改变文件所有者和所属组
[root@cent0S7 master]# chown admin:ad abc.txt 
[root@cent0S7 master]# ll
总用量 0
-rwxrw----. 1 admin ad 0 2月  28 10:20 abc.txt
```

#### 3-chgrp 改变文件目录所属组

**基本语法**

chgrp	[最终用户组]	[文件或目录]

**选项说明**

| 选项 | 说明     |
| ---- | -------- |
| -R   | 递归操作 |

**案例实操**

```shell
#递归修改文件所属组
chgrp -R root /home/admin/
```

 ### 1.6.3 考核内容

- 使用 ll 命令，说明文件权限一列各项参数的含义
- 说明rwx作用于文件和文件夹的区别
- 设置文件权限为属主全部权限、属组读写权限、其他用户无权限
- 改变文件属主信息
- 改变文件属组信息

## 1.7 压缩和解压命令

### 1.7.1 基本命令

#### 1-gzip/gunzip

gzip/gunzip只可解压缩*.gz结尾的文件

**基本语法**

- gzip	待压缩的文件名	（压缩文件）
- gunzip	待解压的文件名	（解压文件）

**案例实操**

```shell
#压缩
[root@localhost admin]# gzip abc.txt 
#解压
[root@localhost admin]# gunzip abc.txt.gz 
```

**经验技巧**

- 只能压缩文件，不能压缩目录
- 不会保留原来的文件
- 可以同时打包多个文件，但会出现多个压缩包

#### 2-zip/unzip

zip/unzip只可解压缩*.zip结尾的文件，zip压缩会保留源文件

**基本语法**

- zip	[选项]	压缩包的文件名.zip	需要打包的文件名称
- unzip    [选项]   压缩包的文件名.zip

**选项说明**

| zip选项 | 功能     |
| ------- | -------- |
| -r      | 压缩目录 |



| unzip选项     | 功能             |
| ------------- | ---------------- |
| -d <目录名称> | 解压至指定的目录 |

**案例实操**

```shell
#压缩目录
[root@localhost home]# zip -r admin.zip admin/
#解压文件至指定目录
[root@localhost home]# unzip -d /home/admin-bak admin.zip 
```

#### 3-tar

tar命令用于解压缩文件，会保留源文件

**基本语法**

tar	[选项]	压缩包文件名.tar.gz	需要打包的文件名称A（可多个）	需要打包的文件名称B ...

**选项说明**

| 选项 | 功能               |
| ---- | ------------------ |
| -c   | 产生.tar打包文件   |
| -v   | 显示详细信息       |
| -f   | 指定压缩后的文件名 |
| -z   | 打包同时压缩       |
| -x   | 解压.tar文件       |
| -C   | 解压到指定目录     |

**案例实操**

```shell
#压缩多个文件
[root@localhost admin]# tar -zcvf abc.tar.gz aaa.txt bbb.txt ccc.txt
#压缩目录
[root@localhost admin]# tar -zcvf admin.tar.gz admin
#解压到当前目录
[root@localhost home]# tar -zxvf admin.tar.gz
#解压到指定目录
[root@localhost home]# tar -zxvf admin.tar.gz admin -C /home/admin-bak 
```

### 1.7.2 考核内容

- 说明解压缩文件有哪几类命令，每类命令用于解压什么格式的文件
- 尝试用tar命令压缩、解压文件

## 1.8 搜索查找命令

### 1.8.1 基本命令

#### 1-find 查找文件或目录

find 命令将从指定目录向下递归遍历各个子目录，将满足条件的文件显示在终端。

**基本语法**

find	[搜索范围]	[选项]

**选项说明**

| 选项  | 说明                                               |
| ----- | -------------------------------------------------- |
| -name | 指定文件名或目录                                   |
| -user | 查找属于指定用户的文件                             |
| -size | 查找指定大小的文件<br/>k-KB<br/>M-MB<br/>G-GB<br/> |

**案例实操**

```shell
#查找指定目录下的文件
[root@localhost home]# find /home/ -name admin*.gz
#查找指定目录下，指定用户的文件
[root@localhost home]# find /home/ -user root
#查找目录下指定大小的文件
[root@localhost home]# find /home/ -size +5M
```

#### 2-locate 快速定位文件路径

locate 会根据系统中所有的文件名称及其路径生成locate数据库，从而实现快速定位文件。locate 指令无需遍历整个文件系统，查询速度较快。

**基本语法**

locate	文件名称

**经验技巧**

- 第一次使用locate命令，必须执行 updatedb 命令创建locate数据库

- locate数据库的更新不是实时的，查找最新的文件前，建议执行 updatedb 命令更新数据库

  **案例实操**

```shell
[root@localhost home]# updatedb
[root@localhost home]# locate admin.zip
/home/admin.zip
```

#### 3-grep 过滤查找及“|”管道符

管道符 “|” 表示将前一个命令的处理结果输出传递给后面的命令处理。

**基本语法**

grep	[选项]	查找内容

**选项说明**

| 选项 | 说明           |
| ---- | -------------- |
| -n   | 显示匹配的行号 |

**案例实操**

```shell
#过滤当前文件夹下包含admin的文件
[root@localhost home]# ll | grep -n admin
3:drwx------. 15 admin admin     4096 Mar 16 02:20 admin
4:drwxr-xr-x.  3 root  root        19 Mar 16 01:13 admin-bak
5:-rw-r--r--.  1 root  root    735498 Mar 20 00:51 admin.tar.gz
6:-rw-r--r--.  1 root  root    734995 Mar 16 01:21 admin.tar.gz1
7:-rw-r--r--.  1 root  root    764453 Mar 16 01:10 admin.zip
```

### 1.8.2 考核内容

- 使用find命令查找文件、查找指定用户的文件、查找指定大小的文件
- 使用locate查找文件，并说明注意事项
- 使用grep和管道符过滤文本中的文件内容

## 1.9 磁盘管理命令

### 1.9.1 基本命令

#### 1-du 查看目录和文件占用的磁盘空间

du: disk usage 磁盘占用情况

**基本语法**

du	目录/文件	[选项]	（功能描述：显示目录下每个子目录的磁盘使用情况）

**选项说明**

| 选项          | 说明                                                 |
| ------------- | ---------------------------------------------------- |
| -h            | 以方便人们进行阅读的方式展示文件大小，如：GB、MB、KB |
| -a            | 显示个别文件或目录的大小                             |
| -c            | 显示文件和目录的总和                                 |
| -s            | 只显示总和                                           |
| --max-depth=n | 指定统计子目录的深度为第n层                          |

**案例实操**

```shell
#查看当前文件夹占用的磁盘空间大小
[root@localhost home]# du -sh
112M	.
#显示指定文件的大小及其总和
[root@localhost home]# du -ac *.zip
748	admin.zip
748	total
#显示第一层文件夹的大小
[root@localhost home]# du -h --max-depth=1
55M	./admin
4.2M	./admin-bak
0	./ad-bak
112M	.
```

#### 2-df 查看磁盘空间使用情况

df：disk free 空余磁盘

**基本语法**

df	[选项]	（功能描述：列出文件系统的整体磁盘使用量，检查文件系统的磁盘空间占用情况）

**选项说明**

| 选项 | 说明                                                 |
| ---- | ---------------------------------------------------- |
| -h   | 以方便人们进行阅读的方式展示文件大小，如：GB、MB、KB |

**案例实操**

```shell
#查看磁盘使用情况
[root@localhost ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        895M     0  895M   0% /dev
tmpfs           910M     0  910M   0% /dev/shm
tmpfs           910M   11M  900M   2% /run
tmpfs           910M     0  910M   0% /sys/fs/cgroup
/dev/sda3        18G  5.2G   13G  30% /
/dev/sda1       297M  163M  134M  55% /boot
tmpfs           182M   12K  182M   1% /run/user/42
tmpfs           182M     0  182M   0% /run/user/0
```

#### 3-lsblk 查看设备挂载情况

**基本语法**

lsblk	[选项]	（功能描述：查看设备挂载情况）

**选项说明**

| 选项 | 说明                                   |
| ---- | -------------------------------------- |
| -f   | 显示更为详细的信息，包括文件系统的信息 |

**案例实操**

```shell
#查看设备挂载情况（boot表示引导分区、SWAP表示内存交换分区、/表示根目录）
[root@localhost ~]# lsblk -f
NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
sda                                                      
├─sda1 xfs          49021d45-f1f2-40e1-87c1-61f7f05f170e /boot
├─sda2 swap         d0c1edb4-5c69-416f-9bfa-3be380d952b4 [SWAP]
└─sda3 xfs          865ac41c-3822-4134-9df6-a9ff41e7ba77 /
sr0 
```

#### 4-mount/umount 挂载/卸载

Linux的文件系统包含多个分区，每个分区对应一个挂载的目录，但整个文件系统都只有一个根目录，根目录下其他的子目录可以来自多个分区。

**基本语法**

mount	[-t vfstype]	[-o options]	挂载设备 挂载点		(功能描述：挂载设备)

umount	设备文件名或挂载点	（功能描述：卸载设备）

**参数说明**

| 参数       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| -t vfstype | 指定文件系统的类型，通常不必指定。mount 会自动选择正确的类型。常用类型有：<br/>光盘或光盘镜像：iso9660<br/>DOS fat16 文件系统：msdos<br/>Windows 9x fat32 文件系统：vfat<br/>Windows NT ntfs 文件系统：ntfs<br/>Mount Windows 文件网络共享：smbfs<br/>UNIX(LINUX) 文件网络共享：nfs |
| -o options | 主要用来描述设备或档案的挂接方式。常用的参数有：<br/>loop：用来把一个文件当成硬盘分区挂接上系统<br/>ro：采用只读方式挂接设备<br/>rw：采用读写方式挂接设备<br/>iocharset：指定访问文件系统所用字符集 |

**案例实操**

(1)挂载光盘镜像文件

```shell
#新建挂载目录
[root@localhost mnt]# mkdir cdrom
#挂载目录
[root@localhost mnt]# mount -t iso9660 /dev/cdrom /mnt/cdrom/
mount: /dev/sr0 is write-protected, mounting read-only
#卸载目录
[root@localhost mnt]# umount /mnt/cdrom 
```

（2）设置开机挂载

```shell
[root@localhost cdrom]# vim /etc/fstab 

UUID=865ac41c-3822-4134-9df6-a9ff41e7ba77 /                       xfs     defaults        0 0
UUID=49021d45-f1f2-40e1-87c1-61f7f05f170e /boot                   xfs     defaults        0 0
UUID=d0c1edb4-5c69-416f-9bfa-3be380d952b4 swap                    swap    defaults        0 0
##新增一行，UUID通过lsblk -f 获取
UUID=2020-11-04-11-36-43-00     /mnt/cdrom      iso9660 defaults        0       0
#此处也可直接配置/dev/cdrom，但更推荐配置UUID,防止重名的设备冲突
#/dev/cdrom     /mnt/cdrom      iso9660 defaults        0       0
```

#### 5-fdisk 分区（仅限2T以内）

**基本语法**

fdisk	-l	（功能描述：查看磁盘分区详情）

fdisk	硬盘设备名	（功能描述：对新增磁盘进行分区操作）

**选项说明**

| 选项 | 说明                   |
| ---- | ---------------------- |
| -l   | 显示所有硬盘的分区列表 |

**功能说明**

- fdisk -l

```shell
[root@localhost ~]# fdisk -l

Disk /dev/sda: 21.5 GB, 21474836480 bytes, 41943040 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x0002026e
#分区序列	#引导分区	#从X磁柱开始	 #从Y磁柱结束 #容量  #分区类型ID  #分区类型		
   Device Boot      Start         End      Blocks   Id  	System
/dev/sda1   *        2048      616447      307200   83  	Linux
/dev/sda2          616448     4810751     2097152   82  	Linux swap / Solaris
/dev/sda3         4810752    41943039    18566144   83  	Linux

Disk /dev/sdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

- fdisk 硬盘设备名

  | 按键 | 说明                 |
  | ---- | -------------------- |
  | m    | 显示命令列表         |
  | p    | 显示当前磁盘分区     |
  | n    | 新增分区             |
  | w    | 写入分区信息并退出   |
  | q    | 不保存分区信息并退出 |

**案例实操**

> **看前须知**
>
> 1.**新增磁盘进行分区的时候，方可参照此方法，操作前请先对服务器建立快照**
>
> **2.进行分区时，若命令操作有误，及时输入 q 退出，不会保存任何更改**
>
> **3.fdisk最大只可创建2T分区的盘，超过2T需使用parted**

1.新增挂载磁盘后，重启服务器才可看到新加的磁盘

```shell
[root@localhost ~]# reboot
```

2.查看新增磁盘的信息

```shell
[root@localhost ~]# fdisk -l

Disk /dev/sda: 21.5 GB, 21474836480 bytes, 41943040 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x0002026e

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048      616447      307200   83  Linux
/dev/sda2          616448     4810751     2097152   82  Linux swap / Solaris
/dev/sda3         4810752    41943039    18566144   83  Linux
###此处为新增磁盘信息，尚未进行分区，磁盘大小为10GB
Disk /dev/sdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

3.开始对磁盘/dev/sdb进行分区

```shell
[root@localhost ~]# fdisk /dev/sdb
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0x8b6fa1f8.

Command (m for help): 
```

4.输入 n ，新增一块分区

```shell
Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
```

5.输入 p ,新增一个主分区

```shell
Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
```

6.连续输入三次回车使用默认参数，确认主分区编号、扇区起始位置、扇区结束位置

```shell
#确认主分区编号
Partition number (1-4, default 1): 
#确认扇区起始位置
First sector (2048-20971519, default 2048): 
Using default value 2048
#确认扇区结束位置
Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519): 
Using default value 20971519
Partition 1 of type Linux and of size 10 GiB is set
```

7.输入 p ，确认新增分区的信息

```shell
Command (m for help): p

Disk /dev/sdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0xc7a8cac9
#确认分区号、起始扇区、结束扇区
   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    20971519    10484736   83  Linux
```

8.输入 w,保存磁盘分区的变更

```shell
Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

9.查看其他挂载磁盘的文件系统类型

```shell
[root@localhost ~]# lsblk -f
NAME   FSTYPE  LABEL           UUID                                 MOUNTPOINT
sda                                                                 
├─sda1 xfs                     49021d45-f1f2-40e1-87c1-61f7f05f170e /boot
├─sda2 swap                    d0c1edb4-5c69-416f-9bfa-3be380d952b4 [SWAP]
└─sda3 xfs                     865ac41c-3822-4134-9df6-a9ff41e7ba77 /
sdb                                                                 
└─sdb1                                                              
```

10.为新划分的分区创建文件系统xfs（根据实际情况指定，最好与之前的分区文件系统保持一致）

```shell
[root@localhost ~]# mkfs -t xfs /dev/sdb1
meta-data=/dev/sdb1              isize=512    agcount=4, agsize=655296 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=2621184, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```

11.将新的分区/dev/sdb1挂载到/data文件下

```shell
[root@localhost ~]# mkdir /data
[root@localhost ~]# mount /dev/sdb1 /data/
[root@localhost ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        895M     0  895M   0% /dev
tmpfs           910M     0  910M   0% /dev/shm
tmpfs           910M   11M  900M   2% /run
tmpfs           910M     0  910M   0% /sys/fs/cgroup
/dev/sda3        18G  5.2G   13G  30% /
/dev/sda1       297M  163M  134M  55% /boot
tmpfs           182M   12K  182M   1% /run/user/42
tmpfs           182M     0  182M   0% /run/user/0
/dev/sdb1        10G   33M   10G   1% /data
```

12.设置开机挂载

```shell
[root@localhost ~]# vim /etc/fstab 
#
# /etc/fstab
# Created by anaconda on Wed Mar  8 08:43:52 2023
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=865ac41c-3822-4134-9df6-a9ff41e7ba77 /                       xfs     defaults        0 0
UUID=49021d45-f1f2-40e1-87c1-61f7f05f170e /boot                   xfs     defaults        0 0
UUID=d0c1edb4-5c69-416f-9bfa-3be380d952b4 swap                    swap    defaults        0 0
#新增一行，UUID通过 lsblk -f 命令获取
UUID=2ba4b44a-aa13-467d-92bd-bd73a7b7456c /data xfs     defaults        0       0
```

#### 6-parted 分区（可处理2T以上）

**基本语法**

parted	硬盘设备名

**案例实操**

> **看前须知**
>
> **1.parted的所有操作都是立即生效，进行操作前一定要做快照！**
>
> **2.parted的所有操作都是立即生效，进行操作前一定要做快照！**
>
> **3.parted的所有操作都是立即生效，进行操作前一定要做快照！**

1.开始分区

```shell
[root@localhost ~]# parted /dev/sdb
GNU Parted 3.1
Using /dev/sdb
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted)    
```

2.修改成 GPT 分区表

```shell
(parted) mklabel
New disk label type? gpt                                                  
Warning: The existing disk label on /dev/sdb will be destroyed and all data on this disk will
be lost. Do you want to continue?
Yes/No? yes  
```

3.建立分区，并设置对应文件系统类型xfs（根据实际情况设置）

```shell
(parted) mkpart                                                           
Partition name?  []? part1                                                
File system type?  [ext2]? xfs                                            
Start? 0%                                                                 
End? 100%
```

4.查看分区信息

```shell
(parted) print                                                            
Model: VMware, VMware Virtual S (scsi)
Disk /dev/sdb: 10.7GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name   Flags
 1      1049kB  10.7GB  10.7GB               part1

```

5.退出parted

```shell
(parted) quit                                                             
Information: You may need to update /etc/fstab.
```

6.建立文件系统

```shell
[root@localhost ~]# mkfs.xfs /dev/sdb1
meta-data=/dev/sdb1              isize=512    agcount=4, agsize=655232 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=2620928, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0

```

7.挂载磁盘

```shell
[root@localhost ~]# mount /dev/sdb1 /data/
```

8.查看磁盘空间使用情况

```shell
[root@localhost ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        895M     0  895M   0% /dev
tmpfs           910M     0  910M   0% /dev/shm
tmpfs           910M   11M  900M   2% /run
tmpfs           910M     0  910M   0% /sys/fs/cgroup
/dev/sda3        18G  5.2G   13G  30% /
/dev/sda1       297M  163M  134M  55% /boot
tmpfs           182M   28K  182M   1% /run/user/0
/dev/sr0        4.4G  4.4G     0 100% /run/media/root/CentOS 7 x86_64
/dev/sdb1        10G   33M   10G   1% /data
```

9.查看新增磁盘分区sdb1的UUID

```shell
#查看设备挂载情况
[root@localhost ~]# lsblk  -f
NAME   FSTYPE  LABEL           UUID                                 MOUNTPOINT
sda                                                                 
├─sda1 xfs                     49021d45-f1f2-40e1-87c1-61f7f05f170e /boot
├─sda2 swap                    d0c1edb4-5c69-416f-9bfa-3be380d952b4 [SWAP]
└─sda3 xfs                     865ac41c-3822-4134-9df6-a9ff41e7ba77 /
sdb                                                                 
└─sdb1 xfs                     7538b2f3-624d-4963-a1e2-6ee023d5152f /data
sr0    iso9660 CentOS 7 x86_64 2020-11-04-11-36-43-00               /run/media/root/CentOS 7
```

10.设置开机自动挂载

```shell

#设置开机自动挂载
[root@localhost ~]# vim /etc/fstab 
# /etc/fstab
# Created by anaconda on Wed Mar  8 08:43:52 2023
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=865ac41c-3822-4134-9df6-a9ff41e7ba77 /                       xfs     defaults        0 0
UUID=49021d45-f1f2-40e1-87c1-61f7f05f170e /boot                   xfs     defaults        0 0
UUID=d0c1edb4-5c69-416f-9bfa-3be380d952b4 swap                    swap    defaults        0 0
#新增一行
UUID=7538b2f3-624d-4963-a1e2-6ee023d5152f       /data   xfs     defaults        0       0

```

#### 7-LVM分区（逻辑卷分区）

LVM（Logical Volume Manager）逻辑卷管理系统，可以对多个磁盘分区建立一个逻辑卷，可弹性调整逻辑卷的大小，每次新增一个磁盘分区至逻辑，只需对新加入的磁盘分区进行格式，不影响之前存储的数据。

**名词解释**

- PV：物理卷，为整个LVM的最底层，为整个物理硬盘的分区，划分了一个特殊区域记录LVM相关的管理参数
- VG：卷组，建立在物理卷之上，一个卷组至少包含一个物理卷，一个逻辑卷可以包含多个卷组
- LV：逻辑卷，逻辑卷建立在卷组之上，卷组中未分配的空间可建立新的逻辑卷，逻辑卷建立后可动态扩展和缩小空间
- PE：物理区域，物理区域是物理卷中的最小存储单元，物理区域在建立物理卷指定，一旦确定无法更改。同一卷组所有物理卷的物理区域大小一致，新的PV加入到VG，PE的大小自动变更为VG中定义的PE大小
- LE：逻辑区域，逻辑区域是逻辑卷中可用于分配的最小存储单元，逻辑区域的大小取决于逻辑卷所在卷组中物理区域的大小。一个逻辑卷最多只能包含65536个PE，所以**PE的大小决定了逻辑卷的容量**，4MB的PE决定了单个逻辑卷的最大容量为256GB。在Red Hat Enterprise Linux AS 4中PE大小范围为8 KB 到 16GB，并且必须总是 2 的倍数

##### 7-0 基本语法

**1.PV物理卷**

**（1）创建物理卷**

pvcreate	[选项]	设备名称

**选项说明**

| 选项 | 说明                         |
| ---- | ---------------------------- |
| -f   | 强制创建逻辑卷，不需用户确认 |
| -u   | 指定设备UUID                 |
| -y   | 所有问题都回答yes            |

**（2）扫描物理卷**

pvscan	[选项]

**选项说明**

| 选项 | 说明                         |
| ---- | ---------------------------- |
| -e   | 仅显示属于输出卷组的物理卷   |
| -n   | 仅显示不属于任何卷组的物理卷 |
| -u   | 显示UUID                     |

**（3）显示物理卷**

pvdisplay	[pv名称]

**（4）移除物理卷**

pvremove	[选项]	pv名称

| 选项 | 说明              |
| ---- | ----------------- |
| -f   | 强制删除          |
| -y   | 所以问题都回答yes |

**2.VG卷组**

**（1）创建卷组**

vgcreate	[选项]	VG名称	PV名称

**选项说明**

| 选项 | 说明                           |
| ---- | ------------------------------ |
| -s   | 卷组中的物理卷PE大小，默认为4M |
| -l   | 卷组上允许创建的最大逻辑卷数   |
| -p   | 卷组中允许添加的最大物理卷数   |

**（2）扫描卷组**

vgscan

**（3）显示卷组**

vgdisplay	[选项]	[VG名称]

**选项说明**

| 选项 | 说明               |
| ---- | ------------------ |
| -A   | 仅显示活动卷组信息 |
| -s   | 使用短格式输出信息 |

**（4）扩容卷组**

vgextend	VG名称	PV名称

**（5）缩容卷组**

vgreduce	VG名称	PV名称	（功能描述：不能移除卷组中最后一个物理卷PV）

**（6）删除卷组**

vgremove	[选项]	VG名称	（功能描述：删除时，其上的逻辑卷必须处于离线状态）

**选项说明**

| 选项 | 说明     |
| ---- | -------- |
| -f   | 强制删除 |

**（7）设置卷组活动状态**

vgchange	-a	n/y	VG名称	（功能描述：-a n 为休眠状态，休眠之前确保其上的逻辑卷都离线；-a y 为活动状态）

**3.LV逻辑卷**

**（1）创建逻辑卷或快照**

lvcreate	[选项]	[参数]

**选项说明**

| 选项 | 说明                                   |
| ---- | -------------------------------------- |
| -L   | 指定大小                               |
| -l   | 指定大小（LE数）                       |
| -n   | 指定名称                               |
| -s   | 创建快照                               |
| -p r | 设置为只读（该选项一般用于创建快照中） |

**参数说明**

使用该命令时，创建逻辑卷参数为指定卷组，创建快照为指定逻辑卷

**（2）扫描逻辑卷**

lvscan

**（3）显示逻辑卷**

lvdisplay	[/dev/VG名称/LV名称]

**（4）扩容逻辑卷**

lvextend	-r -L/-l	扩展的大小	/dev/VG名称/LV名称

**参数说明**

- -L：指定扩展后的大小（单位：M、G）
- -l：指定扩展后的大小（PE数）
- -r:  使扩容的文件系统立即生效

**（5）缩容逻辑卷（离线卸载使用）**

lvreduce	-L/-l	缩减的大小	/dev/VG名称/LV名称

**参数说明**

- -L：指定缩减后的大小（单位：M、G）
- -l：指定缩减后的大小（PE数）

**（6）删除逻辑卷（离线卸载使用）**

lvremove	[选项]	/dev/VG名称/LV名称

**选项说明**

| 选项 | 说明     |
| ---- | -------- |
| -f   | 强制删除 |

##### 7-1 新建逻辑卷

1.查看新挂载的磁盘（本案例新挂载的磁盘为500G的/dev/vdb）

```shell
[root@localhost ~]# fdisk -l
Disk /dev/vda：200 GiB，214748364800 字节，419430400 个扇区
单元：扇区 / 1 * 512 = 512 字节
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：gpt
磁盘标识符：9538B717-B5E7-4EA8-9089-E07530DDFAB8

设备          起点      末尾      扇区   大小 类型
/dev/vda1     2048    411647    409600   200M EFI 系统
/dev/vda2   411648   2508799   2097152     1G Linux 文件系统
/dev/vda3  2508800 419428351 416919552 198.8G Linux LVM


Disk /dev/vdb：500 GiB，536870912000 字节，1048576000 个扇区
单元：扇区 / 1 * 512 = 512 字节
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节


Disk /dev/mapper/klas-root：198.82 GiB，213460713472 字节，416915456 个扇区
单元：扇区 / 1 * 512 = 512 字节
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
```

2.开始分区

```shell
[root@localhost ~]# fdisk /dev/vdb 

欢迎使用 fdisk (util-linux 2.35.2)。
更改将停留在内存中，直到您决定将更改写入磁盘。
使用写入命令前请三思。

设备不包含可识别的分区表。
创建了一个磁盘标识符为 0x7438dd1e 的新 DOS 磁盘标签。
###新建主分区
命令(输入 m 获取帮助)：n
分区类型
   p   主分区 (0 primary, 0 extended, 4 free)
   e   扩展分区 (逻辑分区容器)
选择 (默认 p)：p
分区号 (1-4, 默认  1): 1
第一个扇区 (2048-1048575999, 默认 2048): 2048
最后一个扇区，+/-sectors 或 +size{K,M,G,T,P} (2048-1048575999, 默认 1048575999): 1048575999

创建了一个新分区 1，类型为“Linux”，大小为 500 GiB。
###将分区类型设置为Linux LVM
命令(输入 m 获取帮助)：t
已选择分区 1
Hex 代码(输入 L 列出所有代码)：8e  
已将分区“XENIX root”的类型更改为“Linux LVM”。

###查看新建分区信息
命令(输入 m 获取帮助)：p
Disk /dev/vdb：500 GiB，536870912000 字节，1048576000 个扇区
单元：扇区 / 1 * 512 = 512 字节
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0x7438dd1e

设备       启动  起点       末尾       扇区  大小 Id 类型
/dev/vdb1        2048 1048575999 1048573952  500G 8e Linux LVM

###写入此次操作
命令(输入 m 获取帮助)：w
分区表已调整。
将调用 ioctl() 来重新读分区表。
正在同步磁盘。
```

3.创建物理卷PV

```shell
#pvcreate [分区名称]
[root@localhost ~]# pvcreate /dev/vdb1 
  Physical volume "/dev/vdb1" successfully created.
```

4.创建卷组VG

```shell
#vgcreate [卷组名称] [分区名称]
[root@localhost ~]# vgcreate vg-vdb1 /dev/vdb1 
  Volume group "vg-data" successfully created
```

5.在卷组上创建逻辑卷LV

```shell
#lvcreate -L [逻辑卷大小] -n [逻辑卷名称] [卷组名称]
[root@localhost ~]# lvcreate -L 499.9G -n lv-vdb1 vg-vdb1
  Rounding up size to full physical extent 499.90 GiB
```

6.格式化逻辑卷

```shell
lvscan
mke2fs -t ext4 /dev/data/lv-data
```

7.挂载逻辑卷

```shell
#新建挂载目录
mkdir /data
#挂载设备通过 lsblk -f 查看
#mount [挂载设备] [挂载目录]
mount /dev/mapper/data-lv--data /data
```

8.设置开机挂载

```shell
vim /etc/fstab 

#修改以下内容
#
# /etc/fstab
# Created by anaconda on Mon Jul 17 09:08:44 2023
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
/dev/mapper/klas-root   /                       xfs     defaults        0 0
UUID=b35fde7e-e423-426d-b74c-0bf8e42684b0 /boot                   xfs     defaults        0 0
UUID=4952-D640          /boot/efi               vfat    umask=0077,shortname=winnt 0 2
#新增一行，UUID通过lsblk -f 获取
UUID=77b5a5f1-3556-4522-8740-32395782ad38       /data   ext4    defaults        0 0
```

##### 7-2 逻辑卷扩容

0.格式化新分区

fdisk /dev/vdc  （具体操作不再赘述，参照之前部分）

1.新建磁盘分区创建对应PV

```shell
pvcreate /dev/vdc1
```

2.扩容VG 

```shell
vgextend vg名称  /dev/vdc1
```

3.扩容LV（扩容时指定扩容后大小，并指定-r参数）

```shell
lvextend -r -L 700G  lv名称
```

**经验技巧**

命令去除未知或已丢失的VG

```shell
vgreduce --removemissing /dev/VolGroup00 
```

### 1.9.2 考核内容

- 查看目录占用总大小
- 查看目录类某类文件占用总大小
- 查看磁盘空间使用情况
- 查看设备挂载情况
- 尝试挂载光驱
- 尝试新增一块磁盘进行fdisk分区，并创建文件系统，进行挂载
- 尝试新增一块磁盘进行parted分区，并创建文件系统，进行挂载

## 1.10 进程管理类命令

进程是正在执行的一个程序或命令，每一个进程都是一个运行的实体，都有自己的地址空间，并占用一定的系统资源。

### 1.10.1 基本命令

#### 1-ps 查看当前进程状态

ps：process status	进程状态

**基本语法**

ps	aux	|	grep	xxx	（功能描述：查看系统中所有进程）

ps	-ef	|	grep	xxx	（功能描述：可以查看子父进程之间的关系）

**选项说明**

| 选项 | 说明                                       |
| ---- | ------------------------------------------ |
| a    | 列出带有终端所有用户的进程                 |
| x    | 列出当前用户的偶有进程，包括没有终端的进程 |
| u    | 面向用户友好的显示风格                     |
| -e   | 列出所有进程                               |
| -u   | 列出某个用户关联的所有进程                 |
| -f   | 显示完整格式的进程列表                     |

**功能说明**

1.ps aux	显示信息说明

| 字段      | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| USER      | 该进程是由哪个用户产生的                                     |
| PID       | 进程的 ID 号                                                 |
| %CPU      | 该进程占用 CPU 资源的百分比，占用越高，进程越耗费资源        |
| %MEM      | 该进程占用物理内存的百分比，占用越高，进程越耗费资源         |
| VSZ       | 该进程占用虚拟内存的大小，单位 KB                            |
| RSS       | 该进程占用实际物理内存的大小，单位 KB                        |
| TTY       | 该进程是在哪个终端中运行的。对于 CentOS 来说，tty1 是图形化终端，？表示没有任何终端 |
| tty2-tty6 | 是本地的字符界面终端。pts/0-255 代表虚拟终端                 |
| STAT      | 进程状态。常见的状态有：R：运行状态、S：睡眠状态、T：暂停状态、Z：僵尸状态、s：包含子进程、l：多线程、+：前台显示、<：高优先级、N：低优先级 |
| START     | 该进程的启动时间                                             |
| TIME      | 该进程占用 CPU 的运算时间，注意不是系统时间                  |
| COMMAND   | 产生此进程的命令名                                           |

2.ps -ef	显示信息说明

| 字段  | 说明                                                         |
| ----- | ------------------------------------------------------------ |
| UID   | 用户ID                                                       |
| PID   | 进程 ID                                                      |
| PPID  | 父进程 ID                                                    |
| C     | CPU 用于计算执行优先级的因子。数值越大，表明进程是 CPU 密集型运算，执行优先级会降低；数值越小，表明进程是 I/O 密集型运算，执行优先级会提高 |
| STIME | 进程启动的时间                                               |
| TTY   | 完整的终端名称                                               |
| TIME  | CPU 时间                                                     |
| CMD   | 启动进程所用的命令和参数                                     |

**经验技巧**

- 如果想查看进程的 CPU 占用率和内存占用率，可以使用 aux

- 如果想查看进程的父进程 ID 可以使用 ef

**案例实操**

```shell
#查看进程CPU占用率和内存使用率
[root@localhost data]# ps aux
USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root          1  0.0  0.2 194048  4296 ?        Ss   Apr24   0:25 /usr/lib/systemd/systemd --swit
root          2  0.0  0.0      0     0 ?        S    Apr24   0:00 [kthreadd]
root          4  0.0  0.0      0     0 ?        S<   Apr24   0:00 [kworker/0:0H]
root          6  0.0  0.0      0     0 ?        S    Apr24   0:04 [ksoftirqd/0]
root          7  0.0  0.0      0     0 ?        S    Apr24   0:01 [migration/0]
#查看进程的父进程ID
[root@localhost data]# ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 Apr24 ?        00:00:25 /usr/lib/systemd/systemd --switched-root --syst
root          2      0  0 Apr24 ?        00:00:00 [kthreadd]
root          4      2  0 Apr24 ?        00:00:00 [kworker/0:0H]
root          6      2  0 Apr24 ?        00:00:04 [ksoftirqd/0]
root          7      2  0 Apr24 ?        00:00:01 [migration/0]
#查找ssh进程
[root@localhost data]# ps -ef | grep ssh
root       1078      1  0 Apr24 ?        00:00:00 /usr/sbin/sshd -D
root       8918   8789  0 Apr24 ?        00:00:06 /usr/bin/ssh-agent /bin/sh -c exec -l /bin/bash -c "env GNOME_SHELL_SESSION_MODE=classic gnome-session --session gnome-classic"
root      19897   1078  0 Apr25 ?        00:00:07 sshd: root@pts/1,pts/2,pts/3,pts/0
root      33595  79803  0 14:59 pts/0    00:00:00 grep --color=auto ssh
```

#### 2-kill 终止进程

**基本语法**

kill	[选项]	进程号	（功能描述：通过进程号杀死进程）

killall	进程名称	（功能描述：谨慎使用！！！通过进程名称杀死进程，也支持通配符，在系统因负载过大而变得很慢时可使用）

**选项说明**

| 选项 | 说明                 |
| ---- | -------------------- |
| -9   | 表示强迫进程立即停止 |

**案例实操**

```shell
#根据进程号强制杀死指定进程
[root@localhost data]# kill 19897
#通过进程名称杀死进程
[root@localhost ~]# killall sshd
```

#### 3-pstree 查看进程树

**基本语法**

pstree	[选项]

**选项说明**

| 选项 | 说明               |
| ---- | ------------------ |
| -p   | 显示进程的PID      |
| -u   | 显示进程的所属用户 |

**案例实操**

```shell
#显示进程的PID
[root@localhost ~]# pstree -p
#显示进程的所属用户
[root@localhost ~]# pstree -u
```

#### 4-top 实时监控系统进程状态

**基本语法**

top	[选项]

**选项说明**

| 选项       | 说明                               |
| ---------- | ---------------------------------- |
| -d  秒数   | 指定top命令每隔几秒更新，默认为3秒 |
| -i         | 不显示任何闲置或者僵死进程         |
| -p  进程号 | 指定进程ID来监控进程状态           |

**操作说明**

| 操作 | 说明               |
| ---- | ------------------ |
| P    | 以CPU使用率降序    |
| M    | 以内存使用率降序   |
| N    | 以PID排序          |
| k    | 杀死进程           |
| U    | 筛选指定用户的进程 |
| q    | 退出top            |

**字段说明**

第一行信息为任务队列信息

| 字段                           | 说明                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| top - 08:38:36                 | 系统当前时间                                                 |
| up  2:05                       | 系统运行时间，已运行2小时5分钟                               |
| 1 user                         | 当前登录了2个用户                                            |
| load average: 0.00, 0.01, 0.05 | 分别为系统在1分钟、5分钟、15分钟的CPU平均负载；小于1则负载较小，大于1则系统已超负荷；平均负载小于0.7则代表运行状态良好（以单核CPU估算，多核乘以处理器个数） |

第二行为进程信息

| 字段              | 说明                                      |
| ----------------- | ----------------------------------------- |
| Tasks:  167 total | 系统中的进程总数                          |
| 2 running         | 正在运行的进程数                          |
| 165 sleeping      | 睡眠的进程数                              |
| 0 stopped         | 正在停止的进程数                          |
| 0 zombie          | 僵尸进程，如果不是0，需要手工检查僵尸进程 |

第三行为CPU信息

| 字段             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| %Cpu(s):  0.0 us | 用户模式占用的 CPU 百分比                                    |
| 0.0 sy           | 系统模式占用的 CPU 百分比                                    |
| 0.0 ni           | 改变过优先级的用户进程占用的 CPU 百分比                      |
| 99.9 id          | 空闲 CPU 的 CPU 百分比                                       |
| 0.0 wa           | 等待输入/输出的进程的占用 CPU 百分比                         |
| 0.0 hi           | 硬中断请求服务占用的 CPU 百分比                              |
| 0.0 si           | 软中断请求服务占用的 CPU 百分比                              |
| 0.0 st           | st（Steal time）虚拟时间百分比。就是当有虚拟机时，虚拟 CPU 等待实际 CPU 的时间百分比 |

第四行为物理内存信息

| 字段                     | 说明                    |
| ------------------------ | ----------------------- |
| KiB Mem :  1863032 total | 物理内存的总量，单位 KB |
| 928648 free              | 已经使用的物理内存数量  |
| 551124 used              | 空闲的物理内存数量      |
| 383260 buff/cache        | 作为缓冲的内存数量      |

第五行为交换分区信息

| 字段                     | 说明                         |
| ------------------------ | ---------------------------- |
| KiB Swap:  2097148 total | 交换分区（虚拟内存）的总大小 |
| 2097148 free             | 已经使用的交换分区的大小     |
| 0 used                   | 空闲交换分区的大小           |
| 1133848 avail Mem        | 作为缓存的交换分区的大小     |

第六行表头为进程信息

| 字段    | 说明                           |
| ------- | ------------------------------ |
| PID     | 进程号                         |
| USER    | 进程所属用户                   |
| PR      | 任务调度优先级                 |
| NI      | 任务调度用户指定优先级         |
| VIRT    | 虚拟内存占用大小               |
| RES     | 物理内存占用大小               |
| SHR     | 共享内存占用大小               |
| S       | 进程状态，S代表睡眠，R代表运行 |
| %CPU    | 进程消耗CPU的百分比            |
| %MEM    | 进程消耗内存的百分比           |
| TIME+   | 进行占用CPU运算的总时间        |
| COMMAND | 生成当前进程的命令             |

**案例实操**

```shell
#指定top的刷新时间为10秒
[root@localhost ~]# top -d 10
#过滤闲置和僵死进程
[root@localhost ~]# top -i
#监控指定进程
[root@localhost ~]# top -p 1758
```

#### 5-netstat 显示网络状态和端口占用信息

**基本语法**

netstat  -anp | grep 进程号	（功能描述：查看该进程网络信息）

netstat  -nlp  | grep 端口号	（功能描述：查看网络端口号占用情况）

**选项说明**

| 选项 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| -a   | 显示所有正在监听和未监听的套接字                             |
| -n   | 禁止域名解析功能，套接字以IP地址进行展示，不显示主机名或域名 |
| -l   | 显示所有监听的端口                                           |
| -p   | 表示显示哪个进程在调用                                       |

**案例实操**

```shell
#显示sshd进程的网络信息
[root@localhost ~]# netstat -anp | grep sshd
tcp        0      0 127.0.0.1:6010          0.0.0.0:*               LISTEN      1993/sshd: root@pts 
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1092/sshd           
tcp        0     36 192.168.220.141:22      192.168.220.1:60804     ESTABLISHED 1993/sshd: root@pts 
tcp6       0      0 ::1:6010                :::*                    LISTEN      1993/sshd: root@pts 
tcp6       0      0 :::22                   :::*                    LISTEN      1092/sshd           
unix  2      [ ]         DGRAM                    33304    1993/sshd: root@pts  
unix  3      [ ]         STREAM     CONNECTED     25453    1092/sshd   
#查看22端口的使用情况
[root@localhost ~]# netstat -nlp | grep 22
tcp        0      0 192.168.122.1:53        0.0.0.0:*               LISTEN      1396/dnsmasq        
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1092/sshd           
tcp6       0      0 :::22                   :::*                    LISTEN      1092/sshd           
udp        0      0 192.168.122.1:53        0.0.0.0:*                           1396/dnsmasq  
```

**经验技巧**

麒麟V10操作系统查看端口连接情况

```shell
[root@localhost ~]#  netstat -tn
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       
tcp        0      0 10.20.30.159:49338      10.20.30.249:9685       ESTABLISHED 
tcp        0      0 10.20.30.159:40800      10.20.30.249:9686       ESTABLISHED 
tcp        0    180 10.20.30.159:22         10.20.30.168:56600      ESTABLISHED 
```

#### 6-tcpdump 网络抓包工具

**基本语法**

tcpdump [选项]

**案例实操**

```shell
#监听TCP端口80（HTTP）流量
tcpdump -i eth0 tcp port 80
#监听UDP端口53（DNS）流量
tcpdump -i eth0 udp port 53
#监听TCP或UDP端口80流量
tcpdump -i eth0 port 80
#监听从特定 IP 地址到端口 80 的流量
tcpdump -i eth0 src 192.168.1.100 and tcp port 80

```

#### 7-查看句柄数

合理的句柄数可提高系统并发性能，过多的句柄数会增加系统崩溃的风险

**案例实操**

```shell
#查看系统中所有进程的句柄数
cat /proc/sys/fs/file-nr
#查看指定进程的句柄数，PID为进程ID
lsof -p <PID> | wc -l
#查看句柄数限制
ulimit -n
#查看系统允许的最大句柄数
cat /proc/sys/fs/file-max
```

### 1.10.2 考核内容

- 使用ps命令查找ssh进程，并使用kill命令终止该进程
- 显示进程树
- 讲解top命令中的各项参数的含义，找出CPU和内存占用率最高的进程
- 使用netstat命令查看22端口
- 使用netstat命令查找ssh进程

## 1.11crontab系统定时任务

**前置条件**

验证服务是否启动，未启动则执行`systemctl restart crond`

```shell
[root@localhost ~]# systemctl status crond
```

**基本语法**

crontab	[选项]

**选项说明**

| 选项 | 说明                          |
| ---- | ----------------------------- |
| -e   | 编辑crontab定时任务           |
| -l   | 查询crontab定时任务           |
| -r   | 删除当前用户所有的crontab任务 |

**参数说明**

crontab -e 会打开编辑vim界面，建立定时任务，语法为：* * * * * 要执行的命令

```shell
#每隔一分钟执行一次文本追加
*/1 * * * * echo "hello,world!" >> hello.txt
```

crontab -e 中的任务参数说明

| 参数    | 含义               | 范围                  |
| ------- | ------------------ | --------------------- |
| 第一个* | 一小时中的第几分钟 | 0-59                  |
| 第二个* | 一天当中的第几小时 | 0-23                  |
| 第三个* | 一月当中的第几天   | 1-31                  |
| 第四个* | 一年当中的第几月   | 1-12                  |
| 第五个* | 一周中的星期几     | 0-7（0和7都代表周日） |

特殊符号

| 特殊符号 | 含义                                                         |
| -------- | ------------------------------------------------------------ |
| *        | 代表任何时间，如：第一个*代表每分钟都执行，第二个\*代表每小时都执行 |
| ,        | 代表不连续的时间，如：“0 2,4,6,8 * * * 命令”，表示每天的2点0分、4点0分、6点0分、8点0分各执行一次 |
| -        | 代表连续的时间范围，如：“0 5 * * 1-6 命令”，表示星期一到星期六的每天5点0分执行一次 |
| */n      | 代表每隔多久执行一次，如：“*/10 * * * * 命令”，表示每隔10分钟执行一次 |

注解

@reboot	每次重启的时候执行

参数示例

| 时间              | 含义                                                         |
| ----------------- | ------------------------------------------------------------ |
| 45 22 * * * 命令  | 每天 22 点 45 分执行命令                                     |
| 0 17 * * 1 命令   | 每周 1 的 17 点 0 分执行命令                                 |
| 0 5 1,15 * * 命令 | 每月 1 号和 15 号的凌晨 5 点 0 分执行命令                    |
| 40 4 * * 1-5 命令 | 每周一到周五的凌晨 4 点 40 分执行命令                        |
| */10 4 * * * 命令 | 每天的凌晨 4 点，每隔 10 分钟执行一次命令                    |
| 0 0 1,15 * 1 命令 | 每月 1 号和 15 号，每周 1 的 0 点 0 分都会执行命令（尽量避免同时使用日期和星期，不便于理解） |

## 1.12 软件包管理

### 1.12.1-centOS

**RPM**

RPM（RedHat Package Manager），RedHat软件包管理工具。

RPM包的名称格式，Apache-1.3.23-11.i386.rpm：

- Apache：软件名称
- 1.3.23-11：软件版本号
- i386：软件运行的硬件平台，Inter 32位处理器的统称
- rpm：文件扩展名，代表rpm包

**基本语法**

| 语法                   | 含义                                                         |
| ---------------------- | ------------------------------------------------------------ |
| rpm -qa                | 查询所安装的RPM软件包，一般配合grep使用，如：rmp -qa \| grep ssh |
| rpm -e RPM软件包       | 卸载软件包                                                   |
| rpm -e --nodeps 软件包 | 卸载软件包，忽略依赖关系，会导致依赖此软件的其他软件不可用   |
| rpm -ivh RPM 包全名    | 安装RPM包，-i：表示安装、-v：表示显示详细信息、-h：表示显示进度条，还可以添加 --nodeps：表示安装前不检查依赖 |

**YUM**

YUM是一个在 Fedora 和 RedHat 以及 CentOS中的 Shell 前端软件包管理器。基于 RPM 包管理，能够从指定的服务器自动下载 RPM 包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装

**基本语法**

yum	[选项]	[参数]

**选项说明**

| 选项 | 说明                  |
| ---- | --------------------- |
| -y   | 对所有提问都回答"yes" |

**参数说明**

| 参数         | 说明                          |
| ------------ | ----------------------------- |
| install      | 安装rpm软件包                 |
| update       | 更新rpm软件包                 |
| check-update | 检查是否有可用的更新rpm软件包 |
| remove       | 删除指定的rpm软件包           |
| list         | 显示软件包信息                |
| clean        | 清理yum过期的缓存             |
| deplist      | 显示yum软件包的所有依赖关系   |

**案例实操**

```shell
[root@localhost ~]# yum -y firefox
```

### 1.12.2-ubuntu

## 1.13 防火墙策略

### 1.13.1 centos

#### 1-查看防火墙状态

```shell
# 检查服务是否启动
systemctl status firewalld
# 检查防火墙是否启用
firewall-cmd --state
```

#### 2-开启/关闭防火墙

```shell
# 开启防火墙
systemctl start firewalld
# 关闭防火墙
systemctl stop firewalld
# 重启防火墙
systemctl restart firewalld

# 开启防火墙开机自启
systemctl enable firewalld
# 禁用防火墙开机自启
systemctl disable firewalld

# 重新加载防火墙策略，若添加的端口没有配置【--permanent】，reload后将会失效
firewall-cmd --reload 
# 重新加载防火墙策略,并重启防火墙服务
firewall-cmd --complete-reload 
```

#### 3-防火墙策略配置

防火墙策略修改后，需要重载防火墙策略方可生效

```shell
# --permanent 表示策略永久生效
# --zone 表示策略添加到指定区域，防火墙可划分多个区域，通常配置成public

# 添加80/tcp端口访问策略
firewall-cmd --zone=public --add-port=80/tcp --permanent

# 移除80/tcp端口访问策略
firewall-cmd --zone=public --remove-port=80/tcp --permanent

# 添加80-100端口/tcp访问策略
firewall-cmd --zone=public --add-port=80-100/tcp --permanent

# 允许ip为127.0.0.1访问80/tcp端口
firewall-cmd --add-rich-rule="rule family="ipv4" source address="127.0.0.1" port protocol="tcp" port="80" accept" --permanent

# 允许ip为192.168.3.0网段访问80/tcp端口
firewall-cmd --add-rich-rule="rule family="ipv4" source address="192.168.3.0/24" port protocol="tcp" port="80" accept" --permanent

# 允许ip为192.168.3.0网段访问80-100/tcp端口
firewall-cmd --add-rich-rule="rule family="ipv4" source address="192.168.3.0/24" port protocol="tcp" port="80-100,101,110" accept" --permanent

# 添加拒绝ip为127.0.0.1访问22/tcp端口策略
firewall-cmd --add-rich-rule="rule family="ipv4" source address="127.0.0.1" port protocol="tcp" port="22" reject" --permanent

#移除拒绝ip为127.0.0.1访问22/tcp端口策略
firewall-cmd --remove-rich-rule="rule family="ipv4" source address="127.0.0.1" port protocol="tcp" port="22" reject" --permanent
```

- nfs开放端口2049

#### 4-查看防火墙策略

```shell
# 查看防火墙及添加的端口
firewall-cmd --list-all
# 查看规则设置
firewall-cmd --list-rich-rules
# 查看添加的端口
firewall-cmd --list-ports
# 查看添加的服务
firewall-cmd --list-service
# 查看80端口防火墙策略是否生效
firewall-cmd --query-port=80/tcp
```

**经验技巧**

设置为`--permanent`的防火墙策略，必须执`firewall-cmd --reload `后方可生效；

不设置`--permanent `的防火墙策略会立即生效，重载防火墙后失效；

防火墙添加、修改的规则只有在服务重启后才能查看；

#### 5-考核内容

- 开启防火墙
- 关闭防火墙
- 设置防火墙开机自启
- 重启防火墙
- 重载防火墙配置
- 开放指定端口防火墙策略
- 移除防火墙策略
- 根据端口段开放防火墙策略
- 根据指定IP开放防火墙策略
- 根据指定IP段开放防火墙策略
- 查看防火墙策略

### 1.13.2 ubuntu

#### 1-检查防火墙状态

```shell
#查看防火墙服务
systemctl status ufw
#查看防火墙状态
ufw status
```

#### 2-开启/关闭防火墙

```shell
#开启防火墙服务
systemctl start ufw
#关闭防火墙服务
systemctl stop ufw
#重启防火墙
systemctl restart ufw

#配置防火墙服务开机自启
systemctl enable ufw
#禁用防火墙服务开机自启
systemctl disable ufw

#开启防火墙
ufw enable
#关闭防火墙
ufw disable

#重新加载防火墙规则
ufw reload
#重置防火墙规则为初始状态
ufw reset
```

#### 3-防火墙策略配置

```shell
#限定指定IP访问所有端口
ufw allow from 192.168.3.2 
#限定指定IP访问指定端口
ufw allow from 192.168.3.2 to any port 80

#拒绝指定IP访问所有端口
ufw deny from 192.168.3.2   
#拒绝指定IP访问指定端口
ufw deny from 192.168.3.2 to any port 80

#允许指定网段访问所有端口
ufw allow from 10.20.19.0/24
#允许指定网段访问指定端口
ufw allow from 10.20.19.0/24 to any port 22

#拒绝指定网段访问所有端口
ufw deny from 10.20.19.0/24
#拒绝指定网段访问指定端口
ufw deny from 10.20.19.0/24 to any port 22

#开放端口访问
ufw allow 80
#限制端口访问
ufw deny 80

#删除防火墙策略,根据IP删除(deny可替换成allow)
ufw delete deny from 10.20.19.157
#删除防火墙策略,根据IP和端口删除(deny可替换成allow)
ufw delete deny from 10.20.19.157 to any port 22

#根据防火墙策略编号删除策略
ufw status numbered #查看防火墙策略编号
ufw delete [id] #根据防火墙编号删除策略
```

**经验技巧**

- 添加防火墙策略后，docke服务需要重启，防火墙策略方可生效
- 防火墙策略编号越小，优先级越高

#### 4-考核内容

- 开启防火墙
- 关闭防火墙
- 查看防火墙状态
- 查看防火墙规则
- 设置防火墙开机自启
- 开放指定端口全部IP的访问权限
- 开放指定IP访问指定端口的访问权限
- 根据指定IP段开通端口访问权限
- 禁止指定端口的访问权限
- 禁止指定IP访问指定端口
- 删除防火墙策略

## 1.14 故障排查

### 1.14.1 centos

### 1.14.2 ubuntu

#### 1-ubuntu安装完成磁盘空间只有一半可以使用

1.将整块磁盘分配给对应的磁盘分区，磁盘分区名称通过 lsblk -f 命令查看

```shell
lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
```

2.重新计算磁盘空间大小

```shell
resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

3.查看磁盘空间进行验证

```shell
df -h
```

#### 2-ubuntu设置静态IP

1.修改配置文件/etc/netplan/00-installer-config.yaml，写入以下内容

```shell
network:
  renderer: networkd
  ethernets:
    ens33:
      dhcp4: no
      addresses:
        - 192.168.220.143/24  #静态IP
      gateway4: 192.168.220.2 #网关
      nameservers:
        addresses: [8.8.8.8]
  version: 2
```

2.使配置生效

```shell
netplan apply
```

#### 3-ubuntu启动卡住报错：a start job is running for wait

1.修改配置文件

```shell
vi /etc/systemd/system/network-online.target.wants/systemd-networkd-wait-online.service
```

2.在文件末尾添加`TimeoutStartSec=5sec`

```shell
 
xx@ub:/etc/systemd/system/network-online.target.wants$ cat systemd-networkd-wait-online.service
#  SPDX-License-Identifier: LGPL-2.1-or-later
#
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.
 
[Unit]
Description=Wait for Network to be Configured
Documentation=man:systemd-networkd-wait-online.service(8)
DefaultDependencies=no
Conflicts=shutdown.target
Requires=systemd-networkd.service
After=systemd-networkd.service
Before=network-online.target shutdown.target
 
[Service]
Type=oneshot
ExecStart=/lib/systemd/systemd-networkd-wait-online
RemainAfterExit=yes
TimeoutStartSec=5sec #新添加内容
```

#### 4-禁用systemd-resolved管理DNS

1.停止systemd-resolved服务

```shell
sudo systemctl stop systemd-resolved
```

2.禁用systemd-resolved开机自启

```shell
sudo systemctl disable systemd-resolved
```

3.移除/etc/resolv.conf的符号链接，并创建一个静态的resolv.conf文件

```shell
sudo rm /etc/resolv.conf
sudo echo "nameserver 8.8.8.8" >> /etc/resolv.conf
```

4.重启网络服务应用

```shell
sudo netplan apply
```

#### 5-进入单用户模式（救援模式）

1.开机界面快速按"Esc"进入Grub界面

2.选择”ubuntu“选项，按"e"进行编辑

3.
