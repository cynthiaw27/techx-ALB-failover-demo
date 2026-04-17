# techx-ALB-failover-demo

## Setup Steps
### 1) Launch 2 EC2 Instances 'web-server-1' and 'web-server-2'
- Amazon linux instances
- inbound SSH on port 22
- inbound HTTP on port 80

### 2) SSH into each EC2 instance and install Apache
```bash
sudo dnf update -y
sudo dnf install -y httpd
sudo systemctl enable httpd
sudo systemctl start httpd
```

### 3) Configure server responses
- on 'web-server-1':
  ``` bash
  echo "Server 1" | sudo tee /var/www/html/index.html
  echo "healthy" | sudo tee /var/www/html/health
  ```
- on 'web-server-2'
  ``` bash
  echo "Server 2" | sudo tee /var/www/html/index.html
  echo "healthy" | sudo tee /var/www/html/health
  ```

### 4) Create target group
- target type: instance
- protocol: HTTP
- port: 80
- health check path: /health
- register both EC2 instances as targets

### 5) Create internet-facing ALB
- listener on HTTP port 80
- forwarding rule to the target group

### 6) Verify load balancing
- open ALB DNS name in browser
- refresh to observe alternating responses between 'Server 1' and 'Server 2'

### 7) Test failover
- stop instance 'web-server-1'
- traffic routing to remaining healthy instance 'Server 2'

