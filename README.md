# AltSchool Africa Tinyuka - Cloud Engineering Second Semester Project

## Project Title: The Future of Service-Based Scheduling

**Name**: Abdulsemiu Kusimo

**AltSchool ID**: ALT/SOE/024/2392

**School**: AltSchool Africa Tinyuka

**Track**: Cloud Engineering

---

## Project Overview

This project demonstrates the provisioning of a cloud-based Ubuntu server, configuration of Nginx as a reverse proxy, deployment of a dynamic Node.js web application, and optional SSL encryption. It showcases my ability to manage server infrastructure, secure networking, and modern deployment practices—all designed to simulate a startup-grade prototype.

---

## Project Objectives

* Provision a cloud server
* Configure a production-grade web server (Nginx)
* Deploy a dynamic landing page using Node.js
* Secure the application with HTTPS via Certbot (bonus)
* Use a custom domain: **kusimoclouds.live**

---

## 1. Server Provisioning

### Steps:

**Choose a Cloud Provider or Virtualization Platform:**
Use AWS, GCP, Azure, or a virtualization tool like VirtualBox. 

I chose **AWS**.

**Launch an Instance:**

* Log in to the AWS Management Console.
* Navigate to EC2 Dashboard and click **Launch Instance**.
* Choose an Amazon Machine Image (AMI): Select **Ubuntu 22.04 LTS**.
* Choose an instance type: Select **t2.micro** (Free Tier eligible).

**Configure Instance Details:**

* Leave the default VPC and subnet settings.
* Enable **Auto-assign Public IP**.
* Add storage: Keep the default 8GB or customize as required.

**Configure Security Group:**

* Allow HTTP (port 80) and SSH (port 22).

**Launch the Instance:**

* Download the private key file (e.g., `aws_key.pem`).

**Connect to the Instance:**
Open the terminal and added the `aws_key.pm` file to the `~/.ssh` directory to the directory:

I change the permission of the file using
```bash
chmod 400 aws_key.pem

```
Then I logged into the server using
 
```
ssh -i ~/.ssh/aws_key.pem ubuntu@13.60.40.74

```
---

## 2. Web Server Setup

### Install and Configure Nginx

1. Update the Server:

*Run the following commands:

```bash

sudo apt update && sudo apt upgrade -y

```

2. Install Nginx web server 


```bash
sudo apt install nginx -y

```

### Confirm Nginx is Running

```bash
systemctl status nginx
```

Visit `http://13.60.40.74` in the browser.

### Deploy a Static HTML Page

To deploy a static HTML landing page:

1. **Create Directory Structure**:

   ```bash
   sudo mkdir -p /var/www/kusimoclouds.live/html
   sudo chown -R $USER:$USER /var/www/kusimoclouds.live/html
   sudo chmod -R 755 /var/www/kusimoclouds.live
   ```

2. **Add HTML File**:

   ```bash
   nano /var/www/kusimoclouds.live/html/index.html
   ```
3. **Restart Nginx**:

   ```bash
   sudo nginx -t
   sudo systemctl restart nginx
   ```

### Create Server Block

```bash
sudo mkdir -p /var/www/kusimoclouds.live/html
sudo chown -R $USER:$USER /var/www/kusimoclouds.live/html
sudo chmod -R 755 /var/www/kusimoclouds.live
```

```nginx
server {
    listen 80;
    listen [::]:80;
    root /var/www/kusimoclouds.live/html;
    index index.html index.htm;
    server_name kusimoclouds.live www.kusimoclouds.live;
    location / {
        try_files $uri $uri/ =404;
    }
}
```

Enable site:

```bash
sudo ln -s /etc/nginx/sites-available/kusimoclouds.live /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```


###  How to Reverse Proxy to Node.js App:

**Config File:** `/etc/nginx/sites-available/node_app.conf`

```nginx
server {
    listen 80;
    server_name kusimoclouds.live;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Enable the site:

```bash
sudo ln -s /etc/nginx/sites-available/node_app.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

---

## 4. Networking & Security

* **Ports Opened**: 80 (HTTP), 443 (HTTPS)
* **SSL Certificate**:

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d kusimoclouds.live -d www.kusimoclouds.live
```

* Auto-renewal:

```bash
sudo certbot renew --dry-run
```

---

## Public Access

* **Public IP**: `13.60.40.74`
* **Domain**: [https://kusimoclouds.live](https://kusimoclouds.live)

---

## Screenshots

![Landing Page Screenshot](screenshots/landing_page.png)  


---

## Contact

**Email**:
[abdulsemiu.kusimo@gmail.com](mailto:abdulsemiu.kusimo@gmail.com)
**LinkedIn**:
[linkedin.com/in/abdulsemiu-kusimo](https://linkedin.com/in/abdulsemiu-kusimo)

---


