\# Task 1 — EC2 + Nginx Web Service



\## Overview

Deploy an EC2 instance and provision basic web service and document the deployment process



\## Instance Details

\- Region: us-east-1a (N. Virginia)

\- Instance type: t3.micro

\- Public IPv4: 18.204.34.12



\## Commands Used:



sudo apt update

sudo apt install -y nginx

sudo systemctl start nginx

sudo systemctl enable nginx



\## Service verification steps:



* Service status:

sudo systemctl status nginx --no-pager



* Listening port:

sudo ss -ltnp | grep :80



* Network accessibility:

curl -I http://localhost



\## How traffic reaches the instance:



A browser sends a request to the EC2 public IP.

The security group allows traffic on port 80 (HTTP).

The request reaches the EC2 server.

Nginx receives the request and sends back the web page.

