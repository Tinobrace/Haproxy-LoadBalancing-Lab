===================================HAProxy Load Balancing Project================================


📌 Project Overview

This project demonstrates how to configure HAProxy as a load balancer for Apache2 and Nginx web servers. It implements three load balancing algorithms and includes testing and troubleshooting steps to ensure optimal performance.


🚀 System Architecture

Component	Internal IP	External IP	Port
HAProxy (Load Balancer)	10.128.0.5	35.239.94.162	80
Apache2 Web Server	10.128.0.6	34.136.46.2	80
Nginx Web Server	10.128.0.7	34.31.83.112	80


🔧 Installation & Configuration

1️⃣ Deploy Web Servers

🔹 Apache2 Setup (on apache2-server)
     > sudo apt update && sudo apt install apache2 -y
     > echo "<h1>Apache2 Web Server</h1>" | sudo tee /var/www/html/index.html
     > sudo systemctl restart apache2

🔹 Nginx Setup (on nginx-server)
     > sudo apt update && sudo apt install nginx -y
     > echo "<h1>Nginx Web Server</h1>" | sudo tee /var/www/html/index.html
     > sudo systemctl restart nginx


✅ Verify Setup:
     > curl -I http://10.128.0.6/  # Apache2
     > curl -I http://10.128.0.7/  # Nginx


2️⃣ Install & Configure HAProxy (on haproxy-lb)


🔹 Install HAProxy
     > sudo apt update && sudo apt install haproxy -y

🔹 Configure HAProxy
Edit HAProxy config file
     > sudo nano /etc/haproxy/haproxy.cfg

Paste the following configuration:

global
    stats socket /run/haproxy/admin.sock mode 660 level admin

defaults
    mode http
    timeout connect 5s
    timeout client 30s
    timeout server 30s

frontend http_front
    bind *:80
    use_backend backend_roundrobin if { hdr(host) -i roundrobin.local }
    use_backend backend_leastconn if { hdr(host) -i leastconn.local }
    use_backend backend_iphash if { hdr(host) -i iphash.local }

    # HAProxy Statistics
    stats enable
    stats uri /haproxy?stats
    stats refresh 5s
    stats auth admin:haproxy123

backend backend_roundrobin
    balance roundrobin
    option httpchk GET /
    server web1 10.128.0.6:80 check
    server web2 10.128.0.7:80 check

backend backend_leastconn
    balance leastconn
    option httpchk GET /
    server web1 10.128.0.6:80 check
    server web2 10.128.0.7:80 check

backend backend_iphash
    balance source
    option httpchk GET /
    server web1 10.128.0.6:80 check
    server web2 10.128.0.7:80 check


✅ Restart HAProxy:
    > sudo systemctl restart haproxy


✅ Enable HAProxy at boot:
    > sudo systemctl enable haproxy


✅ Check HAProxy status:
    > sudo systemctl status haproxy


📊 Testing & Validation

1️⃣ Test Load Balancing Algorithms

🔹 Round-Robin
     > curl -I -H "Host: roundrobin.local" http://35.239.94.162/

✅ Expected: Alternates between Apache2 & Nginx.

🔹 Least Connections
     > curl -I -H "Host: leastconn.local" http://35.239.94.162/

✅ Expected: Routes traffic to the server with the fewest active connections.


🔹 IP Hashing
     > curl -I -H "Host: iphash.local" http://35.239.94.162/


✅ Expected: Same client IP is always routed to the same backend.


2️⃣ Load Testing (Apache Benchmark)
     > ab -n 1000 -c 50 -H "Host: roundrobin.local" http://35.239.94.162/

✅ Expected: Requests are distributed across both servers.

3️⃣ Monitor HAProxy Performance

🔹 Check Backend Status
     > echo "show stat" | sudo socat unix-connect:/run/haproxy/admin.sock stdio

🔹 View HAProxy Logs
     > sudo journalctl -xeu haproxy | tail -n 20

🔹 Access HAProxy Web Dashboard
     > http://35.239.94.162/haproxy?stats


✅ Login with:
Username: admin
Password: haproxy123


📌 Final Testing

🔹 Verify Load Balancing

Run multiple requests:

 for i in {1..10}; do curl -I http://haproxy-ip/; done

✅ Expected: Some responses from Apache2, some from Nginx.


📌 Conclusion
✔ HAProxy efficiently balances traffic across Apache2 and Nginx.
✔ Multiple load balancing algorithms ensure high availability.
✔ Health checks keep the system reliable.
✔ Testing & monitoring provide performance insights.
✔ Tested Load Distribution with Apache Benchmark (ab)

📜 Author
👤 Valentine Uchenna Ukah
📧 val.ukah01@gmail.com
📌 GitHub: https://github.com/Tinobrace/Haproxy-LoadBalancing-Lab


