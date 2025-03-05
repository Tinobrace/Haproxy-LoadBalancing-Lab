# HAProxy Performance Comparison Report

1️⃣ Round-Robin Results
ab -n 1000 -c 50 -H "Host: roundrobin.local" http://35.239.94.162/

This is ApacheBench, Version 2.3 <$Revision: 1879490 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 35.239.94.162 (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:        Apache/2.4.52
Server Hostname:        35.239.94.162
Server Port:            80

Document Path:          /
Document Length:        1089 bytes


Concurrency Level:      50
Time taken for tests:   0.206 seconds
Complete requests:      1000
Failed requests:        500


   (Connect: 0, Receive: 0, Length: 500, Exceptions: 0)
Total transferred:      1360500 bytes
HTML transferred:       1087000 bytes
Requests per second:    4848.13 [#/sec] (mean)
Time per request:       10.313 [ms] (mean)
Time per request:       0.206 [ms] (mean, across all concurrent requests)
Transfer rate:          6441.29 [Kbytes/sec] received


Connection Times (ms)


              min  mean[+/-sd] median   max
Connect:        0    1   2.0      1      18
Processing:     1    9   4.0      8      29
Waiting:        1    8   3.8      8      28
Total:          2   10   4.2      9      30


Percentage of the requests served within a certain time (ms)


  50%      9
  66%     10
  75%     11
  80%     12
  90%     14
  95%     20
  98%     25
  99%     26
 100%     30 (longest request)


2️⃣ Least Connections Results
ab -n 1000 -c 50 -H "Host: leastconn.local" http://35.239.94.162/

This is ApacheBench, Version 2.3 <$Revision: 1879490 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 35.239.94.162 (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:        nginx/1.18.0
Server Hostname:        35.239.94.162
Server Port:            80

Document Path:          /
Document Length:        1085 bytes

Concurrency Level:      50
Time taken for tests:   0.193 seconds
Complete requests:      1000
Failed requests:        500
   (Connect: 0, Receive: 0, Length: 500, Exceptions: 0)
Total transferred:      1360500 bytes
HTML transferred:       1087000 bytes
Requests per second:    5194.59 [#/sec] (mean)
Time per request:       9.625 [ms] (mean)
Time per request:       0.193 [ms] (mean, across all concurrent requests)
Transfer rate:          6901.60 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    1   0.7      1       6
Processing:     2    8   2.3      8      17
Waiting:        2    8   2.3      8      17
Total:          4    9   2.2      9      18

Percentage of the requests served within a certain time (ms)
  50%      9
  66%     10
  75%     10
  80%     11
  90%     12
  95%     13
  98%     14
  99%     15
<<<<<<< HEAD
 100%     20 (longest request)
 
=======
 100%     18 (longest request)
>>>>>>> 97ead4d (updates commit - HAProxy Load Balancing Project)


3️⃣ IP Hashing Results
ab -n 1000 -c 50 -H "Host: iphash.local" http://35.239.94.162/

This is ApacheBench, Version 2.3 <$Revision: 1879490 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 35.239.94.162 (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:        nginx/1.18.0
Server Hostname:        35.239.94.162
Server Port:            80

Document Path:          /
Document Length:        1085 bytes

Concurrency Level:      50
Time taken for tests:   0.187 seconds
Complete requests:      1000
Failed requests:        500

   (Connect: 0, Receive: 0, Length: 500, Exceptions: 0)
Total transferred:      1360500 bytes
HTML transferred:       1087000 bytes
Requests per second:    5335.64 [#/sec] (mean)
Time per request:       9.371 [ms] (mean)
Time per request:       0.187 [ms] (mean, across all concurrent requests)
Transfer rate:          7089.00 [Kbytes/sec] received


Connection Times (ms)


              min  mean[+/-sd] median   max
Connect:        0    1   1.0      1       7
Processing:     1    7   6.1      6      40
Waiting:        1    7   6.1      5      40
Total:          2    9   6.3      8      43


Percentage of the requests served within a certain time (ms)
<<<<<<< HEAD
 
  
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
 
=======
  50%      8
  66%     11
  75%     12
  80%     13
  90%     16
  95%     22
  98%     29
  99%     31
 100%     43 (longest request)

>>>>>>> 97ead4d (updates commit - HAProxy Load Balancing Project)

## 🔍 Conclusion  

**Best algorithm for this setup: Least Connections
