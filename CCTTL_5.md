## 🚀 Complete EC2 → Website Hosting Guide (Detailed Step-by-Step) on Amazon Web Services

---

# 🧱 1. Create EC2 Instance (AWS Console)

* Open **AWS Management Console**

* Go to **EC2 Dashboard**

* Click **Launch Instance**

* Fill details:

  * Name: any (e.g., MyServer)
  * OS: **Amazon Linux 2023**
  * Instance type: t2.micro (free tier if available)

* Create **Key Pair**

  * Type: RSA
  * Format: `.pem`
  * Download and save it safely (used for login)

* Configure **Security Group (VERY IMPORTANT)**

  * Allow **SSH (22)** → for terminal login
  * Allow **HTTP (80)** → for website access

* Click **Launch Instance**

* Copy **Public IPv4 address**

---

# 💻 2. Connect to EC2 (FROM LOCAL TERMINAL ONLY)

👉 This step is done in your **Mac/Windows local terminal (NOT AWS terminal)**

* Open terminal
* Go to folder where key is stored:

```bash id="step2a"
cd Downloads
```

* Set correct permissions for key:

```bash id="step2b"
chmod 400 your-key.pem
```

* Connect to EC2:

```bash id="step2c"
ssh -i your-key.pem ec2-user@<public-ip>
```

✔ If successful, terminal changes to:

```id="step2d"
[ec2-user@ip-xxx ~]$
```

👉 Now you are inside **EC2 terminal (remote server)**

---

# 📦 3. Install Apache (INSIDE EC2 TERMINAL)

* Update system (optional but good practice):

```bash id="step3a"
sudo yum update -y
```

* Install Apache web server:

```bash id="step3b"
sudo yum install httpd -y
```

✔ Apache is now installed on EC2

---

# ▶️ 4. Start Apache Service (INSIDE EC2 TERMINAL)

* Start Apache:

```bash id="step4a"
sudo systemctl start httpd
```

* Enable auto-start (optional but recommended):

```bash id="step4b"
sudo systemctl enable httpd
```

* Check status:

```bash id="step4c"
sudo systemctl status httpd
```

✔ Should show: **active (running)**

---

# 🌐 5. Create Simple Website (INSIDE EC2 TERMINAL)

* Go to web root folder:

```bash id="step5a"
cd /var/www/html
```

* Create homepage file:

```bash id="step5b"
echo "<h1>Hello from EC2</h1>" | sudo tee index.html
```

✔ This file becomes your website homepage

---

# 🔄 6. Restart Apache (Recommended)

```bash id="step6"
sudo systemctl restart httpd
```

---

# 🌍 7. Open Website (OUTSIDE TERMINAL – BROWSER)

* Open browser
* Enter:

```
http://<public-ip>
```

✔ You should see:

> Hello from EC2 🚀

---

# 🧠 KEY CONCEPTS (VERY IMPORTANT)

* 🖥️ **Local Terminal**

  * Used only for SSH connection

* ☁️ **EC2 Terminal**

  * Where all Linux commands run
  * Installation + configuration happens here

* 🌐 **Browser**

  * Used to view hosted website

* 📁 **Web Directory**

  * `/var/www/html`
  * Default location for Apache websites

* 🔐 **Key Pair (.pem)**

  * Required for secure login
  * Must use `chmod 400` before SSH

---

# 🎉 FINAL RESULT

You successfully:

* Created EC2 instance
* Connected via SSH
* Installed Apache
* Hosted a live web page

