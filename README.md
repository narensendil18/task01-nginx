# Task 1 - EC2 + Nginx Web Service

## Overview
Deployed an EC2 instance and provision basic web service and documented the deployment process

## Environment
- Region: us-east-1a (N. Virginia)
- Instance type: t3.micro
- Public IPv4: 18.204.34.12

## Commands Used
sudo apt update

## Installing and starting nginx:
sudo apt install -y nginx
sudo systemctl start nginx
sudo systemctl enable nginx

## Service Verification Steps
Service status:
sudo systemctl status nginx --no-pager

Listening port:
sudo ss -ltnp | grep :80

Network accessibility:
curl -I http://localhost

## How Traffic Reaches the Instance
A browser sends a request to the EC2 public IP.
The security group allows traffic on port 80 (HTTP).
The request reaches the EC2 server.
Nginx receives the request and sends back the web page.

# Task 2 - Secure PostgreSQL Deployment on EC2

## Overview

Deploy PostgreSQL server on an EC2 instance with restricted exposure using service configuration and AWS Security Groups.

## Environment

- AWS EC2 (t3.micro)
- Ubuntu 24.04 LTS
- PostgreSQL 16
- Region: us-east-1

## 1. Install PostgreSQL

sudo apt update
sudo apt install -y postgresql postgresql-contrib

## 2. Verify Service is Running

sudo systemctl status postgresql --no-pager
sudo ss -ltnp | grep 5432

Initial output:

127.0.0.1:5432
PostgreSQL was bound to localhost only.

## 3. Remote Connection Test (Expected Failure)

From local machine:
Test-NetConnection <PUBLIC_IP> -Port 5432

Result:
TcpTestSucceeded : False

Remote access failed because PostgreSQL was listening only on 127.0.0.1.

## 4. Enable Controlled Remote Access

Edited configuration:

sudo nano /etc/postgresql/16/main/postgresql.conf

Changed From:

#listen_addresses = 'localhost'

To:

listen_addresses = '*'

Verified:

sudo ss -ltnp | grep 5432

Output:

0.0.0.0:5432
PostgreSQL now listens on all interfaces.

## 5. Restrict Access Using Security Group

Port: 5432

Source: My Public IP (/32)

This ensures only my IP can access the database.

## 6. Network Validation

From local machine:

Test-NetConnection <PUBLIC_IP> -Port 5432

Result:

TcpTestSucceeded : True
Confirms network path is open only for my IP.

## 7. Enable Logging

sudo nano /etc/postgresql/16/main/postgresql.conf

Enabled:

logging_collector = on
log_connections = on
log_disconnections = on

## 8. Monitor Logs in Real Time
sudo tail -f /var/log/postgresql/postgresql-16-main.log

Observed connection activity in real time.
