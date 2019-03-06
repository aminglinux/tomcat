jstack
=========

##### 介绍

jstack主要用来查看Java进程内的线程堆栈信息。

> 注：jstack可以非常方便的做java进程的thread dump，是thread dump的首选方式。

##### 查看帮助信息

可以通过执行jstack 或者 jstack -h来查看帮助信息：

```
$ jstack
Usage:
    jstack [-l] <pid>
        (to connect to running process)
    jstack -F [-m] [-l] <pid>
        (to connect to a hung process)
    jstack [-m] [-l] <executable> <core>
        (to connect to a core file)
    jstack [-m] [-l] [server_id@]<remote server IP or hostname>
        (to connect to a remote debug server)
Options:
    -F  to force a thread dump. Use when jstack <pid> does not respond (process is hung)
    -m  to print both java and native frames (mixed mode)
    -l  long listing. Prints additional information about locks
    -h or -help to print this help message
```

##### 使用场景

1. 连接到运行中的进程

	"jstack pid" --- pid为进程号，可以通过jps或者ps命令查找到。

	命令执行成功则堆栈信息打印在控制台，可以通过 "jstack 4048 > core.dump" 这种方式将内容输出文件。多次dump时可以使用 "jstack 4048 >> core.dump" 将多次dump的内容添加到指定文件。

2. 连接到挂起的进程

	当进程挂起(hung)时，上面的命令可能没有响应，这时需要使用 "-F" 参数来强制执行thread dump： "jstack -F 4048"

3. 分析已有的core文件

	如果已经有java程序崩溃时生成的core文件，那么可以用jstack命令从core文件中提取thread dump信息。

4. 连接到远程调试服务器

	需要和 [jsadebugd](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jsadebugd.html#BHBHHJCA) 工具一起使用。

    > 注：未能测试成功，后续再研究
##### 命令行参数

    -F 当’jstack [-l] pid’没有相应的时候强制打印栈信息
    -l 长列表. 打印关于锁的附加信息,例如属于java.util.concurrent的ownable synchronizers列表.
    -m 打印java和native c/c++框架的所有栈信息.
    -h | -help 打印帮助信息
    pid 需要被打印配置信息的java进程id,可以用jps查询

##### 相关资料

英文资料：

- [jstack官方参考文档(JDK8)](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jstack.html)
- [how the mentioned JStack application works](http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/6-b27/sun/tools/jstack/JStack.java)

中文资料：

- [jstack和线程dump分析](http://jameswxx.iteye.com/blog/1041173)
- [jstack命令(Java Stack Trace)](http://blog.csdn.net/fenglibing/article/details/6411940)
- [JVM性能调优监控工具jps、jstack、jmap、jhat、jstat、hprof使用详解](http://my.oschina.net/feichexia/blog/196575)