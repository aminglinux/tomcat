####jmap命令详解
```
jmap用来在程序运行期间动态查看堆使用信息，以及Java对象信息。

jmap一般适用于以下几种情况：
1) 内存泄漏，线上程序在运行一段时间之后内存越来越大，这是我们要使用jmap命令dump出，内存的对象信息，然后进行分析

2) 内存使用大于预期，这个一般是程序设计不合理，有很多冗余的对象放置在内存中，可以使用jmap查看内存中的对象，看看有些对象是否必要

3) jvm调优，可以使用jmap查看整个堆的使用情况。根据新生代，老年代的大小和使用比利，来对各个区域进行设置。

```

#####jmap -heap 
jmap -heap <pid>  打印堆的摘要信息，包括GC算法，堆的配置信息和使用信息
```
Heap Configuration:                             ##堆配置信息
   MinHeapFreeRatio = 40                        ##在堆的使用率小于40%的时候进行收缩，当Xmx=Xms的时候此配置无效
   MaxHeapFreeRatio = 70                      ##在堆使用率大于70%的时候进行扩展，当Xmx=Xms的时候此配置无效
   MaxHeapSize      = 4294967296 (4096.0MB)     ##堆的最大空间
   NewSize          = 1073741824 (1024.0MB)     ##新生代的大小
   MaxNewSize       = 1073741824 (1024.0MB)     ##最大的新生代的大小
   OldSize          = 2147483648 (2048.0MB)     ##老年代的大小
   NewRatio         = 2                          ##新生代中Eden和和Survivor区的比例为 8:1:1
   SurvivorRatio    = 8                     ##新生代中Eden和和Survivor区的比例为 8:1:1
   PermSize         = 134217728 (128.0MB)       ##永久代的大小
   MaxPermSize      = 134217728 (128.0MB)       ##永久代的最大内存
   G1HeapRegionSize = 0 (0.0MB)                 ##使用G1垃圾收集的区间
Heap Usage:                                     ## 堆的使用信息
New Generation (Eden + 1 Survivor Space):       ##新生代的大小（Eden区加一个Survivor区的空间信息）
   capacity = 966393856 (921.625MB) ##总内存
   used     = 133592616 (127.40384674072266MB)  ##已使用内存
   free     = 832801240 (794.2211532592773MB)   ##剩余内存
   13.823827124993642% used                     ## 使用内存占比
Eden Space:                                     ## Eden区的大小
   capacity = 859045888 (819.25MB)
   used     = 130388888 (124.3485336303711MB)
   free     = 728657000 (694.9014663696289MB)
   15.178337946947952% used
From Space:                                     ##第一个Surivivor区的空间信息
   capacity = 107347968 (102.375MB)
   used     = 3203728 (3.0553131103515625MB)
   free     = 104144240 (99.31968688964844MB)
   2.984432830624237% used
To Space:                                       ##第二个Survivor区的空间信息
   capacity = 107347968 (102.375MB)
   used     = 0 (0.0MB)
   free     = 107347968 (102.375MB)
   0.0% used
concurrent mark-sweep generation:               ##CMS垃圾收集占用的空间信息
   capacity = 3221225472 (3072.0MB)
   used     = 2427667128 (2315.203788757324MB)
   free     = 793558344 (756.7962112426758MB)
   75.36470666527748% used
Perm Generation:                                ##永久代的空间信息
   capacity = 134217728 (128.0MB)
   used     = 63876264 (60.917152404785156MB)
   free     = 70341464 (67.08284759521484MB)
   47.5915253162384% used
```
  
#####jmap -histo
jmap -histo <pid>
按照类进行归纳，该命令会统计Java进程内每个类的对象个数之和，和该类所有对象的占用的空间之和，以及类名称。
注意：
1 该命令在线上执行的时候要做好评估，否则会导致线上机器宕机。
2 jmap -histo:live 这个命令执行，JVM会先触发gc，然后再统计信息。

