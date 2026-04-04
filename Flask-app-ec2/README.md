# 🚀 Deploy a Simple Flask Application on AWS EC2

This project shows how to deploy a basic Flask web application on an AWS EC2 instance.
The app displays a **centered message in large font** on a web page.

---

## 📌 Objective

To create and deploy a simple Flask application that displays **"Your Name"** as a centered heading in the browser.

---

## 🛠️ Technologies Used

* Python 3
* Flask
* AWS EC2 (Linux)
* HTML & CSS

---

## ⚙️ Step 1: Launch EC2 Instance

1. Go to AWS Management Console
2. Open **EC2 Dashboard**
3. Click **Launch Instance**
4. Choose an OS:

   * Amazon Linux 2
   * OR Ubuntu
5. Select instance type: `t2.micro`
6. Configure Security Group:

   * Allow **SSH (22)**
   * Allow **HTTP (80)**

---

## 🔑 Step 2: Connect to EC2

For Amazon Linux:

```bash
ssh -i your-key.pem ec2-user@<public-ip>
```

For Ubuntu:

```bash
ssh -i your-key.pem ubuntu@<public-ip>
```

---

## 🐍 Step 3: Install Python & Flask

### Amazon Linux:

```bash
sudo yum update -y
sudo yum install -y python3 python3-pip
pip3 install flask
```

### Ubuntu:

```bash
sudo apt update -y
sudo apt install -y python3 python3-pip
pip3 install flask
```

### Check & Verify:

```bash
python3 --version
pip3 --version
```

---

## 📝 Step 4: Create Flask App

Create a file:

```bash
nano app.py
```

Paste the following code:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return """
    <html>
        <head>
            <title>Flask App</title>
            <style>
                body {
                    display: flex;
                    flex-direction: column;
                    justify-content: center;
                    align-items: center;
                    height: 100vh;
                    margin: 0;
                    font-family: Arial, sans-serif;
                }
                h1 {
                    font-size: 50px;
                    color: #333;
                    margin: 0;
                }
                h2 {
                    font-size: 30px;
                    color: #555;
                    margin: 10px 0 0 0;
                }
            </style>
        </head>
        <body>
            <h1>Welcome to my Flask App</h1>
            <h3>by Saima Usman - DevOps Architect</h3>
        </body>
    </html>
    """

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=80)

```

---

## 📦 Install the dependencies:

**Create the file:** `requirements.txt`

```bash
nano requirements.txt
```

Write `Flask` and save.

**Run to Download the Dependencies**

```bash
pip3 install -r requirements.txt
```

---

## ▶️ Step 5: Run the Application

```bash
sudo python3 app.py
```

---

## 🌐 Step 6: Access the Application

1. Copy your **Public IPv4 Address**
2. Open browser
3. Visit:

```
http://<your-ec2-public-ip>
```

---

## ✅ Expected Output

You will see:

<img width="1909" height="821" alt="image" src="https://github.com/user-attachments/assets/a0662f2b-236a-46be-8685-d33d8eb01ac4" />


---

## ⚠️ Troubleshooting

* If port 80 fails, try:

```bash
python3 app.py
```

Then open:

```
http://<your-ec2-public-ip>:5000
```

* Ensure HTTP (port 80) is allowed
* Make sure the instance is running

---

## 📚 Learning Outcomes

* Basics of Flask web framework
* Deploying Python apps on EC2
* Running a web server on Linux
* Serving HTML using Flask

---

## 🎉 Conclusion

Successfully deployed a simple Flask web application on AWS EC2 and displayed a styled message in the browser.
