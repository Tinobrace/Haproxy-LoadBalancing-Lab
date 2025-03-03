# HAProxy Performance Comparison Report

## 1️⃣ Round-Robin Results  


Concurrency Level:      50
Time taken for tests:   0.185 seconds
Complete requests:      1000
Failed requests:        500


   (Connect: 0, Receive: 0, Length: 500, Exceptions: 0)
Total transferred:      292500 bytes
HTML transferred:       33500 bytes
Requests per second:    5399.25 [#/sec] (mean)
Time per request:       9.261 [ms] (mean)
Time per request:       0.185 [ms] (mean, across all concurrent requests)
Transfer rate:          1542.27 [Kbytes/sec] received


Connection Times (ms)


              min  mean[+/-sd] median   max
Connect:        0    1   0.7      1       4
Processing:     2    8   2.4      8      18
Waiting:        1    7   2.4      7      17
Total:          4    9   2.2      9      20


Percentage of the requests served within a certain time (ms)


  50%      9
  66%      9
  75%     10
  80%     10
  90%     12
  95%     13
  98%     14
  99%     15
 100%     20 (longest request)
 

## 2️⃣ Least Connections Results  

Concurrency Level:      50
Time taken for tests:   0.190 seconds
Complete requests:      1000
Failed requests:        500

   (Connect: 0, Receive: 0, Length: 500, Exceptions: 0)
Total transferred:      292500 bytes
HTML transferred:       33500 bytes
Requests per second:    5262.91 [#/sec] (mean)
Time per request:       9.500 [ms] (mean)
Time per request:       0.190 [ms] (mean, across all concurrent requests)
Transfer rate:          1503.32 [Kbytes/sec] received


Connection Times (ms)


              min  mean[+/-sd] median   max
Connect:        0    1   0.7      1       4
Processing:     2    8   2.7      8      18
Waiting:        2    8   2.7      7      18
Total:          4    9   2.5      9      19


Percentage of the requests served within a certain time (ms)
 
  
  50%      9
  66%     10
  75%     11
  80%     11
  90%     13
  95%     14
  98%     15
  99%     16
 100%     19 (longest request)


## 3️⃣ IP Hashing Results

  
Concurrency Level:      50
Time taken for tests:   0.195 seconds
Complete requests:      1000
Failed requests:        500

   (Connect: 0, Receive: 0, Length: 500, Exceptions: 0)
Total transferred:      292500 bytes
HTML transferred:       33500 bytes
Requests per second:    5117.45 [#/sec] (mean)
Time per request:       9.771 [ms] (mean)
Time per request:       0.195 [ms] (mean, across all concurrent requests)
Transfer rate:          1461.77 [Kbytes/sec] received


Connection Times (ms)


              min  mean[+/-sd] median   max
Connect:        0    1   0.6      1       4
Processing:     3    8   3.0      8      22
Waiting:        1    8   3.0      8      22
Total:          4    9   2.8      9      23


Percentage of the requests served within a certain time (ms)
  50%      9
  66%     10
  75%     11
  80%     11
  90%     13
  95%     15
  98%     18
  99%     18
 100%     23 (longest request)
 

## 🔍 Conclusion  

**Best algorithm for this setup: Least Connections
