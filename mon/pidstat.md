pidstat是一个功能强大的性能检测工具，安装sysstat包就可以安装该命令。

#### 准备工作

##### 1) 克隆java测试代码
```
# git clone  https://github.com/aminglinux/jvm.action.git
```
##### 2）安装openjdk以及开发工具
```
# yum install -y java-1.8.0-openjdk  java-1.8.0-openjdk-devel-debug
```

#### CPU使用率监控

##### 1）编译java源代码
```
# cd jvm.action/src/main/java
# javac geym/zbase/ch6/hold/HoldCPUMain.java  //该文件为测试CPU使用率的代码
```
##### 2）运行代码
```
# java geym/zbase/ch6/hold/HoldCPUMain   & //把该命令丢到后台去
```  
##### 3）找到java进程pid
```
# jps  //比如pid为13406
13406 HoldCPUMain
```
##### 4）用pidstat分析cpu使用
```
# pidstat -p 13406 -u -t 1 3   //-u表示监控对象为cpu，-t表示显示线程信息，输出内容如下
平均时间:   UID      TGID       TID    %usr %system  %guest    %CPU   CPU  Command
平均时间:     0     13406         -   98.01    0.00    0.00   98.01     -  java
平均时间:     0         -     13406    0.00    0.00    0.00    0.00     -  |__java
平均时间:     0         -     13407    0.00    0.00    0.00    0.00     -  |__java
平均时间:     0         -     13408    0.00    0.00    0.00    0.00     -  |__java
平均时间:     0         -     13409    0.00    0.00    0.00    0.00     -  |__java
平均时间:     0         -     13410    0.00    0.00    0.00    0.00     -  |__java
平均时间:     0         -     13411    0.00    0.00    0.00    0.00     -  |__java
平均时间:     0         -     13412    0.00    0.00    0.00    0.00     -  |__java
平均时间:     0         -     13413    0.00    0.00    0.00    0.00     -  |__java
平均时间:     0         -     13414    0.00    0.00    0.00    0.00     -  |__java
平均时间:     0         -     13415    0.00    0.00    0.00    0.00     -  |__java
平均时间:     0         -     13416   98.01    0.00    0.00   98.01     -  |__java
平均时间:     0         -     13417    0.00    0.00    0.00    0.00     -  |__java
平均时间:     0         -     13418    0.00    0.00    0.00    0.00     -  |__java
平均时间:     0         -     13419    0.00    0.00    0.00    0.00     -  |__java

说明：可以看到id为13416的线程使用cpu最高
```

##### 5）使用jstack导出所有线程
```
# jstack -l 13406 > /tmp/123.txt//查看123.txt，其中有一段内容需要关注
"Thread-0" #8 prio=5 os_prio=0 tid=0x00007f113012d000 nid=0x3468 runnable [0x00007f1135a53000]
   java.lang.Thread.State: RUNNABLE
	at geym.zbase.ch6.hold.HoldCPUMain$HoldCPUTask.run(HoldCPUMain.java:10)
	at java.lang.Thread.run(Thread.java:748)

nid其实就是十六进制的线程id，将0x3468转为10进制，值为13416
所以，要想找到具体的java代码，需要借助pidstat和jstack

```

#### 磁盘IO使用监控

##### 1）编译java源代码
```
# cd jvm.action/src/main/java
# javac geym/zbase/ch6/hold/HoldIOMain.java  //该文件为测试IO使用的代码
```
##### 2）运行代码
```
# java geym/zbase/ch6/hold/HoldIOMain //这个命令会有很多输出内容，需要再开一个终端进行后续操作
```  
##### 3）找到java进程pid
```
# jps
14213 HoldIOMain
14283 Jps
```
##### 4）用pidstat分析cpu使用
```
# pidstat -p 14213 -d -t 1 3  //-d表示监控对象为disk
00时31分01秒   UID      TGID       TID   kB_rd/s   kB_wr/s kB_ccwr/s  Command
00时31分02秒     0     14213         -      0.00    685.15      0.00  java
00时31分02秒     0         -     14213      0.00      0.00      0.00  |__java
00时31分02秒     0         -     14214      0.00      0.00      0.00  |__java
00时31分02秒     0         -     14215      0.00      0.00      0.00  |__java
00时31分02秒     0         -     14216      0.00      0.00      0.00  |__java
00时31分02秒     0         -     14217      0.00      0.00      0.00  |__java
00时31分02秒     0         -     14218      0.00      0.00      0.00  |__java
00时31分02秒     0         -     14219      0.00      0.00      0.00  |__java
00时31分02秒     0         -     14220      0.00      0.00      0.00  |__java
00时31分02秒     0         -     14221      0.00      0.00      0.00  |__java
00时31分02秒     0         -     14222      0.00      0.00      0.00  |__java
00时31分02秒     0         -     14223      0.00    685.15      0.00  |__java
00时31分02秒     0         -     14224      0.00      0.00      0.00  |__java
00时31分02秒     0         -     14225      0.00      0.00      0.00  |__java
00时31分02秒     0         -     14226      0.00      0.00      0.00  |__java
```
