<<<<<<< HEAD
Test Load Distribution


Use Apache Bench (ab) or curl to send requests:

     > ab -n 100 -c 10 http://haproxy-ip/

Collect statistics from HAProxy:

     > echo "show stat" | socat unix-connect:/var/run/haproxy.sock stdio
=======
🚀 Practical Implementation & Testing 

Testing For Apache2 - url -I http://10.128.0.6/ 
HTTP/1.1 200 OK
Date: Wed, 05 Mar 2025 11:30:33 GMT
Server: Apache/2.4.52 (Ubuntu)
Last-Modified: Wed, 05 Mar 2025 08:27:40 GMT
ETag: "441-62f9429a92660"
Accept-Ranges: bytes
Content-Length: 1089
Vary: Accept-Encoding
Content-Type: text/html

Testing For Nginx - curl -I http://10.128.0.7/ 
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Wed, 05 Mar 2025 11:35:14 GMT
Content-Type: text/html
Content-Length: 1085
Last-Modified: Tue, 04 Mar 2025 16:19:44 GMT
Connection: keep-alive
ETag: "67c72820-43d"
Accept-Ranges: bytes

====================================================================================

Testing & Troubleshooting HAProxy Performance 

🔹 Test Each Load Balancing Algorithm with curl 

Round-Robin Test 
curl -I -H "Host: roundrobin.local" http://35.239.94.162/
HTTP/1.1 200 OK
server: nginx/1.18.0 (Ubuntu)
date: Wed, 05 Mar 2025 11:36:31 GMT
content-type: text/html
content-length: 1085
last-modified: Tue, 04 Mar 2025 16:19:44 GMT
etag: "67c72820-43d"
accept-ranges: bytes

Least Connections Test 
curl -I -H "Host: leastconn.local" http://35.239.94.162/
HTTP/1.1 200 OK
date: Wed, 05 Mar 2025 11:38:11 GMT
server: Apache/2.4.52 (Ubuntu)
last-modified: Wed, 05 Mar 2025 08:27:40 GMT
etag: "441-62f9429a92660"
accept-ranges: bytes
content-length: 1089
vary: Accept-Encoding
content-type: text/html

IP Hashing Test 
curl -I -H "Host: iphash.local" http://35.239.94.162/ 
HTTP/1.1 200 OK
server: nginx/1.18.0 (Ubuntu)
date: Wed, 05 Mar 2025 11:39:04 GMT
content-type: text/html
content-length: 1085
last-modified: Tue, 04 Mar 2025 16:19:44 GMT
etag: "67c72820-43d"
accept-ranges: bytes

Load Testing with Apache Benchmark (ab) 

>>>>>>> 97ead4d (updates commit - HAProxy Load Balancing Project)
