####ab压力测试工具
```
1. 最基本的关心两个选项 -c -n
例： ./ab -c 100 -n 10000 http://127.0.0.1/index.php

-c 100 即：每次并发100个
-n 10000 即： 共发送10000个请求

2. 测试结果分析

[junjie2@login htdocs]$ ab -c 1000 -n 50000 "http://10.10.10.10/a.php "
This is ApacheBench, Version 2.3 <$Revision: 655654 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking www.apelearn.com (be patient).....done

Benchmarking 10.65.129.21 (be patient)
Completed 5000 requests
Completed 10000 requests
Completed 15000 requests
Completed 20000 requests
Completed 25000 requests
Completed 30000 requests
Completed 35000 requests
Completed 40000 requests
Completed 45000 requests
Finished 50000 requests
Server Software: Apache/2.2
Server Hostname: 10.65.129.21
Server Port: 80

Document Path: /a.php //请求的资源
Document Length: 0 bytes // 文档返回的长度，不包括相应头

Concurrency Level: 1000 // 并发个数
Time taken for tests: 48.650 seconds //总请求时间 
Complete requests: 50000 // 总请求数
Failed requests: 0 //失败的请求数
Broken pipe errors: 0
Total transferred: 9750000 bytes
HTML transferred: 0 bytes
Requests per second: 1027.75 [#/sec] (mean) // 平均每秒的请求数
Time per request: 973.00 [ms] (mean) // 平均每个请求消耗的时间
Time per request: 0.97 [ms] (mean, across all concurrent requests) // 就是上面的时间 除以并发数
Transfer rate: 200.41 [Kbytes/sec] received // 时间传输速率

Connnection Times (ms)
min mean[+/-sd] median max
Connect: 0 183 2063.3 0 45003
Processing: 28 167 770.6 85 25579
Waiting: 21 167 770.6 85 25578
Total: 28 350 2488.8 85 48639

Percentage of the requests served within a certain time (ms)
50% 85 // 就是有50%的请求都是在85ms内完成的
66% 89
75% 92
80% 96
90% 168
95% 640
98% 984
99% 3203
100% 48639 (last request)
```