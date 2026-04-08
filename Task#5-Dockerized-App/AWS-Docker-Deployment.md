# AWS Docker Deployment - Part-01

This project demonstrates deploying a simple **Dockerized application** using multiple services from Amazon Web Services (AWS), including:

 - EC2
 - AMI
 - ALB
 - ECR (Part-02)
 - ECS (Part-02)
   
The workflow progresses from basic EC2 deployment to a fully managed container deployment.

-----

## Technologies Used

- Amazon Web Services
- Docker Containers
- Amazon EC2
- Amazon ECR (Elastic Container Registry) 
- Amazon ECS (Elastic Container Service)
- ALB (Application Load Balancer)
- AMI (Amazon Machine Image)

----

## Task 1: Deploy Dockerized Web App on EC2

Deploy a Simple Web Page on an EC2 Instance by installing nginx server in a Docker container

### Step 1: Launch EC2 Instance

**By Using Amazon EC2:**

- **AMI:** Amazon Ubuntu 22.04 LTS
- **Instance:** t2.micro
- **Security Group:**
- Port 22 (SSH)
- Port 80 (http access)

-----

### Step 2: Add User Data Script

```bash
#!/bin/bash
apt update -y
apt install -y docker.io
systemctl start docker
systemctl enable docker
usermod -aG docker ubuntu
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

 ## Make your setup AMI-ready, so new instances work WITHOUT any user data

### Step 1: Prepare Your Original Instance (VERY IMPORTANT)

SSH into your working Ubuntu instance:

```batch
ssh -i your-key.pem ubuntu@<IP>
```

### A. Ensure Docker starts on boot

```batch
sudo systemctl enable docker
```

### B. Run Nginx with auto-restart

Stop any old container first

```batch
sudo docker ps    #copy the container id
sudo docker stop <container_id>
sudo docker rm <container_id>
```

### C. Now run it properly:

```batch
sudo docker run -d -p 80:80 --restart unless-stopped \
-v /home/ubuntu/website:/usr/share/nginx/html nginx
```

> --restart unless-stopped = auto-start after reboot AND in AMI


### D. Test before creating AMI

**Reboot instance:**

```batch
sudo reboot
```

**After reboot:**

Open browser → `http://<EC2-IP>`

```batch
You should still see **Hello World!**
```


✅ If yes → ready for AMI\
❌ If not → fix before continuing


----

## Step 2: Create AMI

- Go to EC2 → Instances
- Select your instance
- Click Actions → Image → Create Image

Fill:

- Name: `docker-nginx-ami`
- Description: Docker + Nginx auto-start

Wait until AMI status = `Available`

---

## Step 3: Launch New Instance from AMI

- Go to AMIs
- Select your AMI
- Click Launch instance

⚠️ **Important:**

- Public IP: ✅ Enabled
- Security Group: same (port 80 open)
- User Data: **LEAVE EMPTY**

----

## Step 4: Verify

Open browser:

```bash
http://<NEW-INSTANCE-IP>
```

You should instantly see:

👉 **Hello World!**

-----

**No SSH. No Docker commands. No user data.**


### Why??

- **Docker** is installed in **AMI**
- **Nginx** container already configured
- **--restart unless-stopped** ensures it runs automatically
- Your **HTML** file is already inside the instance

-----

## Task 4: How to Set Up a Load Balancer for Dockerized Applications 

### Objective

To configure an **Application Load Balancer (ALB)** that distributes HTTP traffic across two EC2 instances running Dockerized web applications.

### Step 1: Create an Application Load Balancer

- Go to AWS Console → EC2 → Load Balancers
- Click Create Load Balancer
- Choose Application Load Balancer

#### Configuration:

**Name:** docker-alb
**Scheme:** Internet-facing
**IP type:** IPv4
**Listeners:** HTTP (Port 80)

**Network Mapping:**
 - Select your VPC (Same as before)\
 - Choose at least 2 subnets in different Availability Zones

**Security Group:**
 - Create or select a Security Group with:
  - HTTP (Port 80): 0.0.0.0/0


<img width="1561" height="513" alt="image" src="https://github.com/user-attachments/assets/b5b2f64d-d6be-47e8-b226-aa281453738e" />

----

###  Step 2: Create a Target Group

- Go to EC2 → Target Groups
- Click Create Target Group
- **Configuration:**
 - **Target type:** Instances
- **Protocol:** HTTP
 - Port: 80
- **Target group name:** `docker-target-group`

**Health Check:**
- Protocol: HTTP
- Path: /

Click `Next`

----

### Step 3: Register EC2 Instances

- Select your two EC2 instances
- Click Include as pending below
- Click Create **Target Group**

----

### Step 4: Attach Target Group to Load Balancer

- Go back to Load Balancers
- Select your ALB
- Go to Listeners tab
- Click View/Edit Rules

**Configure:**
Forward traffic to:  →  `docker-target-group`

**Save changes**

----

### Step 5: Verify Load Balancer

- Go to ALB details
- Copy the DNS name

Example:
```batch
http://your-alb-123456.region.elb.amazonaws.com
```

**Open in browser**

<img width="1918" height="363" alt="image" src="https://github.com/user-attachments/assets/48921938-6d67-45bb-8509-6b008c1b7971" />


**After Refresh**

<img width="1919" height="416" alt="image" src="https://github.com/user-attachments/assets/2b6a9e70-a64f-4a70-9805-d7fe9039fa99" />



**Both Targets are Healthy**

<img width="1567" height="442" alt="image" src="https://github.com/user-attachments/assets/e14f6300-ad51-4cf5-95c7-2d59961f00e1" />

----

### Step 6: Verify Load Balancing 

#### Method 1: Modify Each Instance Page 

- SSH into Instance 1:
- See the contents (ls)
- cd /website
- nano index.html
   - <h4>Hello World from EC2-1</h4>

- SSH into Instance 2:
- Repeat the same
  - <h4>Hello World from EC2-2</h4>

- Now refresh the ALB DNS multiple times.

#### Expected Result:

Page alternates between:
- EC2-1
- EC2-2

✅ This confirms load balancing is working.

-----

