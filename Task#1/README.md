# 🚀 Deploy a Simple Web Page on AWS EC2 (Linux)

This project demonstrates how to deploy a simple web page on an AWS EC2 instance using a Linux-based system and a User Data script.

---

## 📌 Objective

To launch an EC2 instance and automatically host a web page displaying **"Hello World"** centered on the screen using Apache Web Server.

---

## 🛠️ Technologies Used

* AWS EC2
* Linux (Amazon Linux 2 / Ubuntu)
* Apache Web Server
* User Data Script
* HTML & CSS

---

## ⚙️ Step 1: Launch EC2 Instance

1. Go to AWS Management Console
2. Navigate to **EC2 Dashboard**
3. Click **Launch Instance**
4. Choose an OS:

   * Amazon Linux 2 *(recommended)*
   * OR Ubuntu
5. Select instance type:

   * `t2.micro` (Free Tier)
6. Configure Security Group:

   * Allow **SSH (22)**
   * Allow **HTTP (80)** ✅
7. Scroll to **Advanced Details**

---

## 📜 Step 2: Add User Data Script

Paste the following script in the **User Data** section:

### 🔹 Amazon Linux 2 Script

```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd

cat <<EOF > /var/www/html/index.html
<html>
<head>
    <title>Hello Page</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            text-align: center;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
</body>
</html>
EOF
```

---

### 🔹 Ubuntu Script

```bash
#!/bin/bash
apt update -y
apt install -y apache2
systemctl start apache2
systemctl enable apache2

cat <<EOF > /var/www/html/index.html
<html>
<head>
    <title>Hello Page</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            text-align: center;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
</body>
</html>
EOF
```

---

## 🚀 Step 3: Launch Instance

* Click **Launch Instance**
* Wait for the instance to reach **Running** state

---

## 🌐 Step 4: Access the Web Page

1. Copy the **Public IPv4 Address**
2. Open a browser
3. Visit:

```
http://<your-ec2-public-ip>
```

---

## ✅ Expected Output

You should see:

> **Hello World**


<img width="1914" height="841" alt="image" src="https://github.com/user-attachments/assets/761e5ec2-8c8c-4dbe-9e95-e6bba4dd937b" />

---

## ⚠️ Troubleshooting

* Ensure **port 80 (HTTP)** is allowed in the security group
* Wait 1–2 minutes after launch for the script to complete
* Verify instance is in **running** state

---

## 📚 Learning Outcomes

* Understanding EC2 instance setup
* Using User Data for automation
* Deploying a basic web server
* Hosting a static web page on Linux

---

## 🎉 Conclusion

Successfully deployed a simple web page on an EC2 instance using a Linux-based system and automated setup via User Data script.
