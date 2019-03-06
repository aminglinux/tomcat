####jstat工具
```
Jstat是JDK自带的一个轻量级小工具。全称“Java Virtual Machine statistics monitoring tool”，
它位于java的bin目录下，主要利用JVM内建的指令对Java应用程序的资源和性能进行实时的命令行的监控，
包括了对Heap size和垃圾回收状况的监控。可见，Jstat是轻量级的、专门针对JVM的工具，非常适用。

jstat工具特别强大，有众多的可选项，详细查看堆内各个部分的使用量，以及加载类的数量。
使用时，需加上查看进程的进程id，和所选参数。
```

#####jstat -class <pid> 显示加载class的数量，及所占空间等信息。

| 列名       | 说明    |
| :--------   | :-----   | 
|loaded       |装载类的数量|
|bytes|装载类所占用的字节数|
|unloaded|卸载类的数量|
|bytes|卸载类的字节数|
|time|装载和卸载类所花费的时间|
  
```
# jstat -class -t 35522 1000 2
Timestamp       Loaded  Bytes   Unloaded   Bytes     Time   
     51.4       389      865.1        0     0.0       0.06
     52.8       389      865.1        0     0.0       0.06
说明： -t可以显示timestamp列，即显示时间戳，1000 2表示每隔1000毫秒（即1s）打印一次信息，共打印两次
```
#####jstat -compiler <pid>显示VM实时编译的数量等信息。

| 列名       | 说明    |
| :--------   | :-----   | 
|Compiled      |编译任务执行数量|
|Failed|编译任务执行失败数量|
|Invalid  |编译任务执行失效数量 |
|Time   |编译任务消耗时间 |
|FailedType|最后一个编译失败任务的类型|
|FailedMethod|最后一个编译失败任务所在的类及方法|

```
# jstat -compiler  35522
Compiled Failed Invalid   Time   FailedType FailedMethod
     160      0       0     0.33          0 
```

#####jstat -gc <pid>: 可以显示gc的信息，查看gc的次数，及时间。

| 列名       | 说明    |
| :--------   | :-----   | 
|S0C   |年轻代中第一个survivor（幸存区）的容量 (字节)|
|S1C   |年轻代中第二个survivor（幸存区）的容量 (字节)|
|S0U   |年轻代中第一个survivor（幸存区）目前已使用空间 (字节)|
|S1U     |年轻代中第二个survivor（幸存区）目前已使用空间 (字节)|
|EC      |年轻代中Eden（伊甸园）的容量 (字节)|
|EU       |年轻代中Eden（伊甸园）目前已使用空间 (字节)|
|OC|Old代的容量 (字节)|
|OU      |Old代目前已使用空间 (字节)|
|PC    |Perm(持久代)的容量 (字节)|
|PU|Perm(持久代)目前已使用空间 (字节)|
|YGC    |从应用程序启动到采样时年轻代中gc次数|
|YGCT   |从应用程序启动到采样时年轻代中gc所用时间(s)|
|FGC   |从应用程序启动到采样时old代(全gc)gc次数|
|FGCT    |从应用程序启动到采样时old代(全gc)gc所用时间(s)|
|GCT|从应用程序启动到采样时gc用的总时间(s)|
```
# jstat -gc 35522
 S0C    S1C    S0U    S1U      EC       EU        OC         OU       MC     MU    CCSC   CCSU   YGC     YGCT    FGC    FGCT     GCT   
1024.0 1024.0  0.0   1024.0  8192.0   4838.3   20480.0     2647.2   4864.0 2803.6 512.0  260.7       1    0.016   0      0.000    0.016
```

#####jstat -gccapacity <pid>:可以显示，VM内存中三代（young,old,perm）对象的使用和占用大小
| 列名       | 说明    |
| :--------   | :-----   | 
|NGCMN   |年轻代(young)中初始化(最小)的大小(字节)|
|NGCMX    |年轻代(young)的最大容量 (字节)|
|NGC    |年轻代(young)中当前的容量 (字节)|
|S0C  |年轻代中第一个survivor（幸存区）的容量 (字节)|
|S1C      |年轻代中第二个survivor（幸存区）的容量 (字节)|
|EC     |年轻代中Eden（伊甸园）的容量 (字节)|
|OGCMN     |old代中初始化(最小)的大小 (字节)|
|OGCMX      |old代的最大容量(字节)|
|OGC|old代当前新生成的容量 (字节)|
|OC     |Old代的容量 (字节)|
|PGCMN   |perm代中初始化(最小)的大小 (字节)|
|PGCMX    |perm代的最大容量 (字节)  |
|PGC      |perm代当前新生成的容量 (字节)|
|PC    |Perm(持久代)的容量 (字节)|
|YGC   |从应用程序启动到采样时年轻代中gc次数|
|FGC|从应用程序启动到采样时old代(全gc)gc次数|
  
```
# jstat -gccapacity 35522
 NGCMN    NGCMX     NGC     S0C   S1C       EC      OGCMN      OGCMX       OGC         OC       MCMN     MCMX      MC     CCSMN    CCSMX     CCSC    YGC    FGC 
 10240.0 156288.0  10240.0 1024.0 1024.0   8192.0    20480.0   312704.0    20480.0    20480.0      0.0 1056768.0   4864.0      0.0 1048576.0    512.0      1     0
```
  
#####jstat -gcutil <pid>:统计gc信息
| 列名       | 说明    |
| :--------   | :-----   | 
|S0    |年轻代中第一个survivor（幸存区）已使用的占当前容量百分比|
|S1    |年轻代中第二个survivor（幸存区）已使用的占当前容量百分比|
|E     |年轻代中Eden（伊甸园）已使用的占当前容量百分比|
|O     |old代已使用的占当前容量百分比|
|P    |perm代已使用的占当前容量百分比|
|YGC|从应用程序启动到采样时年轻代中gc次数|
|YGCT   |从应用程序启动到采样时年轻代中gc所用时间(s)|
|FGC   |从应用程序启动到采样时old代(全gc)gc次数|
|FGCT    |从应用程序启动到采样时old代(全gc)gc所用时间(s)|
|GCT|从应用程序启动到采样时gc用的总时间(s)|
  
#####jstat -gcnew <pid>:年轻代对象的信息。
| 列名       | 说明    |
| :--------   | :-----   | 
|S0C   |年轻代中第一个survivor（幸存区）的容量 (字节)|
|S1C   |年轻代中第二个survivor（幸存区）的容量 (字节)|
|S0U   |年轻代中第一个survivor（幸存区）目前已使用空间 (字节)|
|S1U  |年轻代中第二个survivor（幸存区）目前已使用空间 (字节)|
|TT|持有次数限制|
|MTT |最大持有次数限制|
|EC      |年轻代中Eden（伊甸园）的容量 (字节)|
|EU    |年轻代中Eden（伊甸园）目前已使用空间 (字节)|
|YGC    |从应用程序启动到采样时年轻代中gc次数|
|YGCT|从应用程序启动到采样时年轻代中gc所用时间(s)|
  
  
#####jstat -gcnewcapacity<pid>: 年轻代对象的信息及其占用量。
| 列名       | 说明    |
| :--------   | :-----   | 
|NGCMN     |年轻代(young)中初始化(最小)的大小(字节)|
|NGCMX      |年轻代(young)的最大容量 (字节)|
|NGC     |年轻代(young)中当前的容量 (字节)|
|S0CMX    |年轻代中第一个survivor（幸存区）的最大容量 (字节)|
|S0C    |年轻代中第一个survivor（幸存区）的容量 (字节)|
|S1CMX    |年轻代中第二个survivor（幸存区）的最大容量 (字节)|
|S1C      |年轻代中第二个survivor（幸存区）的容量 (字节)|
|ECMX|年轻代中Eden（伊甸园）的最大容量 (字节)|
|EC     |年轻代中Eden（伊甸园）的容量 (字节)|
|YGC |从应用程序启动到采样时年轻代中gc次数|
|FGC|从应用程序启动到采样时old代(全gc)gc次数|
  
#####jstat -gcold <pid>：old代对象的信息
| 列名       | 说明    |
| :--------   | :-----   | 
|PC      |Perm(持久代)的容量 (字节)|
|PU       |Perm(持久代)目前已使用空间 (字节)|
|OC         |Old代的容量 (字节)|
|OU      |Old代目前已使用空间 (字节)|
|YGC   |从应用程序启动到采样时年轻代中gc次数|
|FGC   |从应用程序启动到采样时old代(全gc)gc次数|
|FGCT    |从应用程序启动到采样时old代(全gc)gc所用时间(s)|
|GCT|从应用程序启动到采样时gc用的总时间(s)|
  
#####stat -gcoldcapacity <pid>: old代对象的信息及其占用量
| 列名       | 说明    |
| :--------   | :-----   | 
|OGCMN      |old代中初始化(最小)的大小 (字节)|
|OGCMX       |old代的最大容量(字节)|
|OGC        |old代当前新生成的容量 (字节)|
|OC      |Old代的容量 (字节)|
|YGC  |从应用程序启动到采样时年轻代中gc次数|
|FGC   |从应用程序启动到采样时old代(全gc)gc次数|
|FGCT    |从应用程序启动到采样时old代(全gc)gc所用时间(s)|
|GCT|从应用程序启动到采样时gc用的总时间(s)|
  
#####stat -gcpermcapacity<pid>: perm对象的信息及其占用量
|列名       | 说明    |
| :--------   | :-----   | 
|PGCMN     |perm代中初始化(最小)的大小 (字节)|
|PGCMX      |perm代的最大容量 (字节)  |
|PGC        |perm代当前新生成的容量 (字节)|
|PC     |Perm(持久代)的容量 (字节)|
|YGC  |从应用程序启动到采样时年轻代中gc次数|
|FGC   |从应用程序启动到采样时old代(全gc)gc次数|
|FGCT    |从应用程序启动到采样时old代(全gc)gc所用时间(s)|
|GCT|从应用程序启动到采样时gc用的总时间(s)|
  
#####jstat -printcompilation <pid>：当前VM执行的信息
|列名       | 说明    |
| :--------   | :-----   | 
|Compiled |编译任务的数目|
|Size |方法生成的字节码的大小|
|Type|编译类型|
|Method|类名和方法名用来标识编译的方法。类名使用/做为一个命名空间分隔符。方法名是给定类中的方法。上述格式是由-XX:+PrintComplation选项进行设置的|