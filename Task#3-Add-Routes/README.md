# 🚀 Task 3: Add Routes to the Flask Application

This project demonstrates how to extend a Flask web application by **adding multiple routes**.
Each route displays a **centered, readable message** using HTML and CSS.

---

## 📌 Objective

To create a Flask app with multiple pages:

* Homepage (`/`)
* About (`/about`)
* Contact (`/contact`)
* Status (`/status`)

Each page displays a **centered message** in large font.

---

## 🛠️ Technologies Used

* Python 3
* Flask
* AWS EC2 (Linux)
* HTML & CSS

---

## ⚙️ Step 1: Launch EC2 Instance

1. Go to AWS Management Console
2. Navigate to **EC2 Dashboard**
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

```bash id="q0x8gm"
ssh -i your-key.pem ec2-user@<public-ip>
```

For Ubuntu:

```bash id="v1x9kt"
ssh -i your-key.pem ubuntu@<public-ip>
```

---

## 📦 Step 3: Install Python & Flask

### Amazon Linux:

```bash id="f6v01b"
sudo yum update -y
sudo yum install -y python3 python3-pip
pip3 install flask
```

### Ubuntu:

```bash id="r5k21p"
sudo apt update -y
sudo apt install -y python3 python3-pip
pip3 install flask
```

---

## 📝 Step 4: Create Flask App

Create a file:

```bash id="u9d7qv"
nano app.py
```

Paste the following code:

```python id="flaskroutes"
from flask import Flask

app = Flask(__name__)

def generate_page(title, message):
    return f"""
    <html>
        <head>
            <title>{title}</title>
            <style>
                body {{
                    display: flex;
                    flex-direction: column;
                    justify-content: center;
                    align-items: center;
                    height: 100vh;
                    margin: 0;
                    font-family: Arial, sans-serif;
                }}
                h1 {{
                    font-size: 40px;
                    color: #333;
                    margin: 0 0 20px 0;
                }}
                p {{
                    font-size: 24px;
                    color: #555;
                    margin: 0;
                }}
            </style>
        </head>
        <body>
            <h1>{title}</h1>
            <p>{message}</p>
        </body>
    </html>
    """

@app.route('/')
def home():
    return generate_page("Homepage", "Welcome to the Homepage!")

@app.route('/about')
def about():
    return generate_page("About", "This is a simple Flask application.")

@app.route('/contact')
def contact():
    return generate_page("Contact", "Contact us at: contact@devops.com")

@app.route('/status')
def status():
    return generate_page("Status", "The app is running perfectly!")

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

## 🌐 Step 6: Access the Routes

Open your browser and visit:

| Route      | Output                                                                      |
| ---------- | --------------------------------------------------------------------------- |
| `/`        | Homepage: "Welcome to the Homepage!"                                        |
| `/about`   | About: "This is a simple Flask application."                                |
| `/contact` | Contact: "Contact us at: [contact@example.com](mailto:contact@example.com)" |
| `/status`  | Status: "The app is running perfectly!"                                     |

Example:

```text
http://<your-ec2-public-ip>/about
```

---

## ✅ Expected Output

- **/**

<img width="1909" height="807" alt="image" src="https://github.com/user-attachments/assets/935d7edf-6e07-4c7a-b00b-e370f5aaedfa" />
<br>

-  **/about**

<img width="1910" height="799" alt="image" src="https://github.com/user-attachments/assets/9d5ef2f4-9cf7-428d-a124-b37a5364a232" />
<br>

 - **/contact**

<img width="1918" height="805" alt="image" src="https://github.com/user-attachments/assets/2cf7e33d-f954-4dd1-b389-ec832a8c2878" />

 - **/status**

<img width="1919" height="854" alt="image" src="https://github.com/user-attachments/assets/1b5ea857-514c-4325-bcb5-3aeb68a5f868" />

---

## ⚠️ Troubleshooting

* If port 80 fails, try port 5000

```bash
python3 app.py
```

Then open:

```text
http://<your-ec2-public-ip>:5000
```

* Ensure **HTTP (port 80)** is allowed
* Verify the instance is running

---

## 📚 Learning Outcomes

* Created multiple Flask routes
* Served dynamic HTML content through Python Flask
* Deployed Python apps on Linux EC2

---

## 🎉 Conclusion

Successfully added multiple routes to a Flask application on EC2, each displaying **styled, centered messages** for easy readability.
