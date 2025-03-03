Test Load Distribution


Use Apache Bench (ab) or curl to send requests:

     > ab -n 100 -c 10 http://haproxy-ip/

Collect statistics from HAProxy:

     > echo "show stat" | socat unix-connect:/var/run/haproxy.sock stdio
