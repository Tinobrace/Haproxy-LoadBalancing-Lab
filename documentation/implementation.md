                           ==========Detailed Configuration Explanations==========


This project sets up HAProxy as a load balancer for two web servers (Apache2 & Nginx) on Google Cloud Platform (GCP).




📌 Step 1: Create Three VMs on GCP

1️⃣ Web Server 1 (apache2-server)

2️⃣ Web Server 2 (nginx-server)

3️⃣ Load Balancer (haproxy-lb)


Firewall Rules:

Allow HTTP traffic (port 80)
Allow internal traffic between VMs


📌 Step 2: Set Up the Web Servers

✅ Apache2 (Port 8000)

Run on apache2-server:
     > sudo apt update && sudo apt install apache2 -y
     > sudo nano /etc/apache2/ports.conf
     

Modify to:

     > Listen 0.0.0.0:8000
     

Edit Virtual Host:

     > sudo nano /etc/apache2/sites-enabled/000-default.conf
     

Modify:

     > <VirtualHost *:8000>
           DocumentRoot /var/www/html
       </VirtualHost>
       
       
Create a test page:

     > echo "<h1>Apache Server</h1>" | sudo tee /var/www/html/index.html
    > sudo systemctl restart apache2


✅ Nginx (Port 80)

Run on nginx-server:

     > sudo apt update && sudo apt install nginx -y
     > echo "<h1>Nginx Server</h1>" | sudo tee /var/www/html/index.html
     > sudo systemctl restart nginx
     
     
📌 Step 3: Install & Configure HAProxy

Run on haproxy-lb:

     > sudo apt update && sudo apt install haproxy -y
     
     
Edit HAProxy config:

     > sudo nano /etc/haproxy/haproxy.cfg

     
Paste:

     > global
         stats socket /run/haproxy/admin.sock mode 660 level admin

     defaults
         log global
         mode http
         timeout connect 5s
         timeout client 30s
         timeout server 30s

     frontend http_front
         bind *:80
         default_backend web_servers

     backend web_servers
         balance roundrobin
         option httpchk GET /
         server web1 10.128.0.2:8000 check
         server web2 10.128.0.3:80 check

         
Restart HAProxy:

     > sudo systemctl restart haproxy
     
     
📌 Step 4: Test Load Balancing

1️⃣ Check HAProxy Response:

     > curl -I http://haproxy-ip/
     
Each request should alternate between Apache2 and Nginx.


2️⃣ Access HAProxy Stats Page:

Go to:

      http://haproxy-ip/haproxy?stats

      
Login:

Username: admin
Password: haproxy123


📌 Step 5: Load Testing with Apache Benchmark (ab)

Install Apache Benchmark (ab):

     > sudo apt install apache2-utils -y
     
Run Load Test:

     > ab -n 100 -c 10 http://haproxy-ip/
     
     
🚀 Final Summary


✅ Created 3 VMs (2 Web Servers + 1 HAProxy Load Balancer)

✅ Configured Apache2 on Port 8000 & Nginx on Port 80

✅ Installed & Configured HAProxy with Load Balancing

✅ Tested Load Distribution & HAProxy Monitoring



🎯 Now HAProxy is successfully balancing traffic between Apache2 & Nginx! 



🚀 Note that:

Nginx is hosted on 10.128.0.3 (Internal IP) or 34.136.46.2 (External IP)

Apache2 is hosted on 10.128.0.2 (Internal IP) or 35.238.73.183 (External IP)

Haproxy is hosted on 10.128.0.4 (Internal IP) or 35.239.94.162 (External IP)
