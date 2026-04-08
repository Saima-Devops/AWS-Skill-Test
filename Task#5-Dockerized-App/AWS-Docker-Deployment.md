# AWS Docker Deployment Project

This project demonstrates deploying a **Dockerized Node.js application** using multiple services from Amazon Web Services (AWS), including:

 - EC2,
 - AMI,
 - ALB,
 - ECR, and
 - ECS
   
The workflow progresses from basic EC2 deployment to a fully managed container deployment using ECS.

-----

## Technologies Used

- Amazon Web Services
- Docker Containers
- Amazon EC2
- Amazon ECR (Elastic Container Registry) 
- Amazon ECS (Elastic Container Service)
- ALB (Application Load Balancer)
- AMI (Amazon Machine Image)



-----

## Task 1: Deploy Dockerized Web App on EC2

Deploy a Simple Web Page on an EC2 Instance by installing nginx server in a Docker container

### Step 1: Launch EC2 Instance

**By Using Amazon EC2:**

- AMI: Amazon Ubuntu 22.04 LTS
- Instance: t2.micro
- Security Group:
- Port 22 (SSH)
- Port 80 (http access)

-----

### Step 2: Add User Data Script

```bash
#!/bin/bash
# -----------------------------
# Ubuntu EC2 Startup Script
# -----------------------------

# Update packages
apt update -y
apt upgrade -y

# Install Docker
apt install -y docker.io

# Start Docker and enable on boot
systemctl start docker
systemctl enable docker

# Add ubuntu user to docker group
usermod -aG docker ubuntu

# Create website directory and custom index.html
mkdir -p /home/ubuntu/website
echo "<html><h1>Hello, World!</h1></html>" > /home/ubuntu/website/index.html

# Pull Nginx image
docker pull nginx

# Run Nginx container with host port 80, mounting custom website
# --restart unless-stopped ensures container starts automatically on reboot
docker run -d -p 80:80 --restart unless-stopped -v /home/ubuntu/website:/usr/share/nginx/html nginx
```
-----

### Step 3: Verify in Browser

**Open:**

```bash
http://<PUBLIC-IP>
```

You should see `Nginx` Welcome Page
<br>

<img width="1919" height="648" alt="image" src="https://github.com/user-attachments/assets/a9d36418-3aea-4e49-9d43-5a54126c590b" />

----

## Task 2:  Create and Use a Custom AMI with Docker Pre-installed 

### Step 1: Create Custom AMI with already prepared Instance

- Select the specific dockerized instance
- Change the sgtate to `STOP`
- **Actions** → **Image** → **Create Image**
- Reboot Instance ✅
- Create a new instance from this AMI

----

### Step 2: Launch Instance from AMI

- Go to AMIs
- Launch new instance
- Ensure:
  - Public IP is checked ✅
  - Same Security Group

<img width="1919" height="456" alt="image" src="https://github.com/user-attachments/assets/67dd5112-af87-4e73-8936-21408d8ecede" />

----

### Step 3: SSH & Verify

```bash
ssh -i your-key.pem ubuntu@<NEW-IP>
```

**Check Docker:**

```bash
docker --version
systemctl status docker
docker run -d -p 80:80 nginx
```

----

### Step 4: Verify in Browser

**Open:**

```bash

http://<PUBLIC-IP>:80

```

You should see the `Nginx` Welcome Page here

----

## Important to Note:

### Nginx Page on New Instance

If your AMI already contains the Docker container running Nginx (for example, you ran it and then created the AMI), the container itself does not persist between reboots or AMI creation.
This means when you launch a new instance from the AMI, the container will not automatically be running. You need one of the following:

#### Option 1: Use systemd or Docker restart policy

Before creating the AMI, run your container with a restart policy:

```bash
docker run -d -p 80:80 --restart unless-stopped nginx
```

This ensures Docker restarts the container automatically when the instance boots.

#### Option 2: Use user data to start the container

Add to the new instance’s user data:

The same data as in task#1.

✅ Then, after the instance boots, your Nginx welcome page will be visible on 

```bash
http://<PUBLIC-IP>
```

---

## Practical Recommendation

- Build AMI after installing Docker and adding your website
- Use Docker restart policy so containers start automatically
- No need to copy old user data, but you can add extra commands if needed
- Launch second instance from the AMI → after boot, Nginx will be accessible

----

## Task 3: How to create a custom welcome page and store in a directory  

During launching a new EC2 instance, paste the following user data script, and it will:

- Install Docker
- Start Docker on boot
- Create a custom “Hello, World!” page
- Run an Nginx container serving your page, auto-starting on reboot 


```batch
#!/bin/bash
# -----------------------------
# Ubuntu EC2 Startup Script
# -----------------------------

# Update packages
apt update -y
apt upgrade -y

# Install Docker
apt install -y docker.io

# Start Docker and enable on boot
systemctl start docker
systemctl enable docker

# Add ubuntu user to docker group
usermod -aG docker ubuntu

# Create website directory and custom index.html
mkdir -p /home/ubuntu/website
echo "<html><h1>Hello World!</h1></html>" > /home/ubuntu/website/index.html

# Pull Nginx image
docker pull nginx

# Run Nginx container with host port 80, mounting custom website
# --restart unless-stopped ensures container starts automatically on reboot
docker run -d -p 80:80 --restart unless-stopped -v /home/ubuntu/website:/usr/share/nginx/html nginx
```

---

## How it works:

- **Docker installation:** Installs Docker if not already present.
- **Startup on boot:** systemctl enable docker ensures Docker starts on every reboot.
- **Custom website:** Your /home/ubuntu/website/index.html is mounted inside the Nginx container.
- **Persistent Nginx container:** --restart unless-stopped ensures it stays running even after instance reboots.

---

**Note:** This script also works if you create a custom AMI from this instance — any new instance launched from that AMI will already have 
Docker installed, and Nginx will auto-start serving your page.

-----

