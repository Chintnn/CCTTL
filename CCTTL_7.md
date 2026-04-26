Got it 👍 I’ll give you a **full “from zero to working system” Assignment 7 guide** with **no missing steps**, including the **temporary Apache fix you used** on Amazon Web Services.

---

# 🚀 Assignment 7: Auto Scaling + Load Balancer (FULL DETAILED NOTES)

---

# 🧱 1. Create EC2 Launch Template (BASE FOR ASG)

👉 Purpose: Defines how every EC2 instance is created

---

## 🔹 Steps:

* Go to **EC2 Dashboard**
* Click **Launch Templates**
* Click **Create launch template**

---

## 🔹 Basic Configuration:

* Name → `asg-template`
* AMI → **Amazon Linux 2023**
* Instance Type → `t2.micro`
* Key Pair → select existing `.pem`
* Security Group:

  * Allow **SSH (22)**
  * Allow **HTTP (80)**

---

## 🔹 Network Settings:

* Choose → **Don’t include in launch template**

---

## 🔹 Storage:

* Keep default

---

## 🔹 Create Template

✔ Launch Template created (Version 1)

---

# ⚙️ 2. Create Auto Scaling Group (ASG)

👉 Purpose: Automatically manages EC2 instances

---

## 🔹 Steps:

* Go to **EC2 → Auto Scaling Groups**
* Click **Create Auto Scaling Group**

---

## 🔹 Step 1: Choose Launch Template

* Select → `asg-template`
* Version → 1

---

## 🔹 Step 2: Name ASG

* Name → `my-asg`

---

## 🔹 Step 3: Network

* VPC → Default VPC
* Subnets:

  * Select **2 or more subnets**
  * Must be in **different AZs**

---

## 🔹 Step 4: Load Balancer Section (IMPORTANT PART YOU MISSED EARLIER)

👉 Select:

* “Attach to an existing load balancer”

👉 Then:

* Choose **Application Load Balancer**
* Select **Target Group (you will create next)**

⚠️ If you don’t attach here → ALB won’t work

---

## 🔹 Step 5: Group Size

* Minimum → 1
* Desired → 2
* Maximum → 5

---

## 🔹 Step 6: Scaling Policy

* Select → **No scaling policy**

---

## 🔹 Step 7: Health Checks

* Enable → EC2 health checks

---

## 🔹 Step 8: Tags

* Key → Name
* Value → AutoScaling-Instance
* Enable → **Propagate at launch**

---

## 🔹 Create ASG

✔ ASG created successfully

---

# ⚖️ 3. Create Target Group

👉 Purpose: Receives traffic from Load Balancer

---

## 🔹 Steps:

* Go to **EC2 → Target Groups**
* Click **Create target group**

---

## 🔹 Configuration:

* Target type → **Instances**
* Protocol → HTTP
* Port → 80
* VPC → same as ASG

---

## 🔹 Health Check:

* Path → `/`

---

## 🔹 Register Targets:

👉 IMPORTANT (your confusion earlier)

✔ DO NOT select anything here
✔ Leave empty
✔ Click **Create**

👉 Reason:
ASG will automatically register instances

---

✔ Target Group created

---

# ⚖️ 4. Create Application Load Balancer (ALB)

---

## 🔹 Steps:

* Go to **EC2 → Load Balancers**
* Click **Create Load Balancer**
* Choose → Application Load Balancer

---

## 🔹 Configuration:

* Name → `my-alb`
* Scheme → Internet-facing
* IP type → IPv4

---

## 🔹 Network:

* Select:

  * Default VPC
  * **2 public subnets (different AZs)**

---

## 🔹 Security Group:

* Allow HTTP (80) from anywhere

---

## 🔹 Listener:

* HTTP → port 80

---

## 🔹 Default Action:

* Forward to → **Target Group**

---

## 🔹 Create Load Balancer

✔ ALB created successfully

---

# 🔗 5. Attach ALB to ASG (IF NOT DONE EARLIER)

---

## 🔹 Steps:

* Go to **Auto Scaling Groups**
* Select ASG
* Click **Edit**
* Attach:

  * Load Balancer → ALB
  * Target Group → your TG
* Save

---

# 💥 6. IMPORTANT ISSUE YOU FACED (FIXED HERE)

## ❌ Problem:

* Instances were **unhealthy**
* `httpd.service not found`

---

## ⚡ TEMPORARY FIX (YOU USED THIS)

👉 SSH into EACH EC2 instance:

```bash id="fix1"
ssh -i key.pem ec2-user@<instance-ip>
```

---

### Install Apache:

```bash id="fix2"
sudo yum install httpd -y
```

---

### Start Apache:

```bash id="fix3"
sudo systemctl start httpd
```

---

### Enable Apache:

```bash id="fix4"
sudo systemctl enable httpd
```

---

### Create simple page:

```bash id="fix5"
cd /var/www/html
echo "<h1>Healthy Instance</h1>" | sudo tee index.html
```

---

### Restart:

```bash id="fix6"
sudo systemctl restart httpd
```

---

✔ This fixes “unhealthy target”

---

# 🔍 7. Verify Target Group Health

* Go to **Target Groups**
* Open **Targets tab**

✔ Status should be:

* 🟢 healthy

---

# 🌐 8. Test Load Balancer DNS

---

## 🔹 Step:

* Go to **Load Balancers**
* Copy **DNS name**

Example:

```id="dns1"
my-alb-123456.us-east-1.elb.amazonaws.com
```

---

## 🔹 Open in browser:

```id="dns2"
http://<alb-dns>
```

✔ Website should load

---

# 🔄 9. Test Auto Scaling

---

## ✔ Scale Out:

* Set desired → 3 or 4
  ✔ New instances launch

---

## ✔ Scale In:

* Set desired → 1
  ✔ Instances terminate

---

## ✔ Auto Healing:

* Terminate instance
  ✔ ASG creates new one

---

# 🎯 FINAL RESULT (WRITE THIS)

* Launch Template created successfully
* Auto Scaling Group configured
* Target Group created and linked
* Application Load Balancer configured
* Instances launched automatically
* Apache manually installed (temporary fix)
* Load Balancer DNS tested
* High availability achieved
* Auto Scaling verified

---

# 🧠 KEY LEARNINGS (IMPORTANT FOR VIVA)

* Launch Template = blueprint
* ASG = manages EC2 count
* Target Group = receives traffic
* ALB = distributes traffic
* Health check failure = Apache missing
* Temporary fix = install httpd manually
* Best practice = use User Data (future improvement)


If you want next, I can:
👉 Turn this into **perfect PDF assignment format (ready to submit)**
👉 Or make **viva questions + answers from this assignment** 👍
