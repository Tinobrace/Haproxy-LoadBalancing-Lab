                 ==========📖 HAProxy Load Balancing Lab – Project Documentation==========


This document provides a detailed step-by-step guide for setting up HAProxy as a load balancer for two web servers (Apache2 & Nginx), including configuration, testing, and troubleshooting.


📌 Project Overview

🎯 Goal:

Deploy HAProxy as a load balancer for two web servers (Apache2 on port 8000 and Nginx on port 80).

Implement three load-balancing algorithms:
Round-Robin
Least Connections
IP Hash
Enable health checks to ensure traffic is routed to only available servers.
Configure HAProxy stats monitoring.
Test load distribution using ApacheBench (ab).
🛠️ Technologies Used:
HAProxy – Load Balancer
Apache2 – Web Server (port 8000)
Nginx – Web Server (port 80)
Ubuntu 22.04 (GCP VM) – Operating System
Apache Benchmark (ab) – Load Testing Tool
socat – HAProxy socket communication
📌 Infrastructure Setup
🌍 1. Create Virtual Machines on GCP
We created three VMs:
1️⃣ HAProxy Load Balancer (haproxy-lb)
2️⃣ Apache2 Web Server (apache2-server)
3️⃣ Nginx Web Server (nginx-server)

💡 Each server was assigned an internal IP to communicate within the same VPC.

📌 Web Server Configuration
🔹 2. Install & Configure Apache2 (Port 8000)
Run on apache2-server:

bash
Copy
Edit
sudo apt update
sudo apt install apache2 -y
🔹 Set Apache to listen on port 8000:

bash
Copy
Edit
sudo nano /etc/apache2/ports.conf
Change:

plaintext
Copy
Edit
Listen 0.0.0.0:8000
Listen [::]:8000
🔹 Edit Apache Virtual Host Configuration

bash
Copy
Edit
sudo nano /etc/apache2/sites-enabled/000-default.conf
Modify:

apache
Copy
Edit
<VirtualHost *:8000>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html
</VirtualHost>
🔹 Restart Apache2:

bash
Copy
Edit
sudo systemctl restart apache2
🔹 Verify Apache2 is Running on Port 8000

bash
Copy
Edit
curl -I http://localhost:8000/
✅ Expected Output:

Copy
Edit
HTTP/1.1 200 OK
🔹 3. Install & Configure Nginx (Port 80)
Run on nginx-server:

bash
Copy
Edit
sudo apt update
sudo apt install nginx -y
🔹 Edit Nginx Configuration to Serve a Custom Page

bash
Copy
Edit
sudo nano /var/www/html/index.html
Paste:

html
Copy
Edit
<h1>Nginx Web Server</h1>
🔹 Restart Nginx

bash
Copy
Edit
sudo systemctl restart nginx
🔹 Verify Nginx is Running

bash
Copy
Edit
curl -I http://localhost/
✅ Expected Output:

Copy
Edit
HTTP/1.1 200 OK
📌 HAProxy Configuration
🔹 4. Install & Configure HAProxy
Run on haproxy-lb:

bash
Copy
Edit
sudo apt update
sudo apt install haproxy -y
🔹 Edit HAProxy Configuration

bash
Copy
Edit
sudo nano /etc/haproxy/haproxy.cfg
Paste:

plaintext
Copy
Edit
global
    log /dev/log local0
    log /dev/log local1 notice
    stats socket /run/haproxy/admin.sock mode 660 level admin
    user haproxy
    group haproxy
    daemon

defaults
    log global
    mode http
    option httplog
    timeout connect 5s
    timeout client 30s
    timeout server 30s
    retries 3

frontend http_front
    bind *:80
    mode http
    default_backend web_servers

    # HAProxy Statistics
    stats enable
    stats uri /haproxy?stats
    stats refresh 5s
    stats auth admin:haproxy123

# Load Balancing Algorithms

# 1️⃣ Round-Robin
backend backend_roundrobin
    balance roundrobin
    option httpchk GET /health
    server web1 10.128.0.2:8000 check
    server web2 10.128.0.3:80 check

# 2️⃣ Least Connections
backend backend_leastconn
    balance leastconn
    option httpchk GET /health
    server web1 10.128.0.2:8000 check
    server web2 10.128.0.3:80 check

# 3️⃣ IP Hashing
backend backend_iphash
    balance source
    option httpchk GET /health
    server web1 10.128.0.2:8000 check
    server web2 10.128.0.3:80 check
🔹 Restart HAProxy

bash
Copy
Edit
sudo systemctl restart haproxy
✅ Test Load Balancer

bash
Copy
Edit
curl -I http://haproxy-ip/
✅ Expected Output:

Copy
Edit
HTTP/1.1 200 OK
📌 Load Testing HAProxy
🔹 5. Install Apache Benchmark (ab)
bash
Copy
Edit
sudo apt install apache2-utils -y
🔹 6. Run Load Test
bash
Copy
Edit
ab -n 100 -c 10 http://haproxy-ip/
✅ Expected Output:

sql
Copy
Edit
Requests per second: 500 [#/sec]
Time per request: 10ms
🔹 Verify HAProxy Load Distribution

bash
Copy
Edit
echo "show stat" | sudo socat unix-connect:/run/haproxy/admin.sock stdio
📌 HAProxy Stats Monitoring
View HAProxy stats in a browser:

arduino
Copy
Edit
http://haproxy-ip/haproxy?stats
✅ Login:

Username: admin
Password: haproxy123
📌 Final Testing
🔹 Verify Load Balancing
Run multiple requests:

bash
Copy
Edit
for i in {1..10}; do curl -I http://haproxy-ip/; done
✅ Expected: Some responses from Apache2, some from Nginx.

📌 Project Summary
✔ Deployed HAProxy as a Load Balancer
✔ Implemented Round-Robin, LeastConn, and IP Hash
✔ Configured Health Checks
✔ Set Up HAProxy Monitoring
✔ Tested Load Distribution with Apache Benchmark (ab)

