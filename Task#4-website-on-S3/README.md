# 🚀 Task 4: Host a Static Website Using Amazon S3

This project demonstrates how to host a **simple static website** using **Amazon S3** without complex configurations.

---

## 📌 Objective

To create and deploy a static website that displays a **centered message** using HTML & CSS, hosted on Amazon S3.

---

## 🛠️ Technologies Used

* AWS S3
* HTML & CSS

---

## ⚙️ Step 1: Create an S3 Bucket

1. Go to AWS Management Console
2. Open **S3** service
3. Click **Create Bucket**
4. Enter a unique name (e.g., `my-portfolio-site`)
5. Select a region
6. Important: Under Block Public Access settings, **uncheck** “Block all public access”❌
7. Leave default settings
8. Click **Create Bucket**

---

## ⚙️ Step 2: Enable Static Website Hosting

1. Open your bucket
2. Go to **Properties** tab
3. Scroll to **Static website hosting**
4. Click **Enable**
5. Enter:

   * Index document: `index.html`
6. Click **Save changes**

---

## 📦 Step 3: Upload Website File

1. Go to **Objects** tab
2. Click **Upload → Add files**
3. Select your `index.html`
4. Click **Upload**

---

## 🌍 Step 4: Make File Public

1. Click on `index.html`
2. Click **Object Actions → Make public**
3. Confirm

---

## 🌐 Step 5: Access Your Website

1. Go to **Properties → Static website hosting**
2. Copy the **Endpoint URL**
3. Open it in your browser

---

## 💻 index.html 

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Portfolio</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            font-family: Arial, sans-serif;
            background-color: #f7f7f7;
            text-align: center;
        }
        h1 {
            font-size: 50px;
            color: #333;
            margin-bottom: 20px;
        }
        p {
            font-size: 24px;
            color: #555;
            margin: 0;
        }
    </style>
</head>
<body>
    <h1>Welcome to My Portfolio!</h1>
    <p>This is a simple static website hosted on Amazon S3.</p>
</body>
</html>
```

---

## ✅ Expected Output

* Website is accessible via S3 endpoint

<img width="1919" height="824" alt="image" src="https://github.com/user-attachments/assets/3093a469-3a48-4a60-a036-4d996915f3cf" />


---

## ⚠️ Troubleshooting

* If page doesn’t load:

  * Make sure `index.html` is **public**
  * Check Static Website Hosting is **enabled**
  * Wait a few seconds after upload

---
 
## 📚 Learning Outcomes

* Understand S3 bucket creation
* Host a static website on AWS
* Use HTML & CSS for styling
* Manage file permissions in S3

---

## 🎉 Conclusion

Successfully deployed a **static website on Amazon S3** using a beginner-friendly approach without bucket policy errors.
