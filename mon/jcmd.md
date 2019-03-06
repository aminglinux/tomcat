#### jcmd命令使用
jcmd是jdk1.7以及以后版本多出来的一个工具，它是可以全能工具，可以导出堆、查看java进程、导出线程信息、执行GC等。

1 列出所有java进程
```
# jcmd -l 
48416 sun.tools.jcmd.JCmd -l
37895 org.apache.catalina.startup.Bootstrap start
```

2 列出可用指令
```
# jcmd 37895 help
37895:
The following commands are available:
VM.native_memory
ManagementAgent.stop
ManagementAgent.start_local
ManagementAgent.start
VM.classloader_stats
GC.rotate_log
Thread.print
GC.class_stats
GC.class_histogram
GC.heap_dump
GC.finalizer_info
GC.heap_info
GC.run_finalization
GC.run
VM.uptime
VM.dynlibs
VM.flags
VM.system_properties
VM.command_line
VM.version
```

3 查看虚拟机启动时间
```
# jcmd 37895 VM.uptime 
37895:
11656.548 s
```

4 打印线程栈信息
```
# jcmd 37895 Thread.print
```

5 查看系统中类的统计信息
```
# jcmd 37895 GC.class_histogram
```

6 导出堆信息
```
# jcmd 37895 GC.heap_dump  /tmp/123.dump  //这里的123.dump是二进制文件，需要通过特定工具查看（MAT）
```

7 获取JVM启动参数
```
# jcmd 37895 VM.flags
```

8 获取系统Properties
```
# jcmd 37895 VM.system_properties
```
 
9 获取所有性能相关的数据
```
# jcmd 37895 PerfCounter.print
```