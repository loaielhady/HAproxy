# haproxy
is a free, open-source software that provides high availability, load balancing, and proxying for TCP and HTTP-based applications. It is widely used to improve the performance and reliability of web applications by distributing incoming traffic across multiple servers.
### steps to set up haproxy load balancing:
we have two node js applications running on two different ports, and we want to set up HAProxy to load balance the traffic between them.

#### applications 1:
```
const http = require('http');
http.createServer((req, res) => {
  res.end('This signal from server A');
}).listen(7001);
```

#### applications 2:

```
const http = require('http');
http.createServer((req, res) => {
  res.end('This signal from server B');
}).listen(7002);
```
1. Install HAProxy:
   - On Ubuntu/Debian: `sudo apt-get install haproxy`
    - On CentOS/RHEL: `sudo yum install haproxy`
### 2. Configure HAProxy:
   - Edit the HAProxy configuration file, typically located at `/etc/haproxy/haproxy.cfg`
   - Define the frontend and backend sections. For example:
     
     ```
     frontend http_front
         bind *:500
         timeout client 60s
         mode http
         default_backend http_back
     backend http_back
         timeout connect 60s
         timeout server 60s
         mode http
         server server1 127.0.0.1:7001 check
         server server2 127.0.0.1:7002 check 
### 3. Start and enable HAProxy:
   - Start the HAProxy service: `sudo systemctl start haproxy`
   - Enable the HAProxy service to start on boot: `sudo systemctl enable haproxy`
### 4. Test the configuration:
   - Check the status of HAProxy: `sudo systemctl status haproxy`
   - Test the load balancing by accessing the HAProxy server's IP address in a web browser.
    You should see the responses from the backend servers being distributed.
