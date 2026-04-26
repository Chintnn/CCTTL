## 🚀 Assignment 6: Auto Scaling (Complete Step-by-Step Guide) on Amazon Web Services

---

# 🧱 1. Create Launch Template (VERY IMPORTANT BASE)

👉 Launch Template = **blueprint for EC2 instances**

---

## 🔹 Steps:

* Open **AWS Management Console**
* Go to **EC2 Dashboard**
* Left sidebar → **Launch Templates**
* Click **Create launch template**

---

## 🔹 Fill Details:

* **Launch template name** → `auto-scale-template`
* **Template version description** → optional

---

## 🔹 AMI (OS Selection):

* Click **Browse AMIs**
* Select → **Amazon Linux 2023**

👉 Defines OS for all auto-created instances

---

## 🔹 Instance Type:

* Select → `t2.micro`

👉 Free tier + lightweight

---

## 🔹 Key Pair:

* Select your existing key (e.g., `ccttl-key`)

👉 Needed if you want to SSH into instances later

---

## 🔹 Security Group:

* Select existing security group
* Must allow:

  * **SSH (22)**
  * **HTTP (80)**

---

## 🔹 Network Settings:

* Select → **Don’t include in launch template**

👉 Important → ASG will handle networking

---

## 🔹 Storage:

* Leave default (8 GB gp3)

---

👉 Click **Create launch template**

✔ Output: Launch Template created (Version 1)

---

# ⚙️ 2. Create Auto Scaling Group (ASG)

👉 ASG = automatically manages number of EC2 instances

---

## 🔹 Navigation:

* Go to **EC2 Dashboard**
* Click **Auto Scaling Groups**
* Click **Create Auto Scaling Group**

---

# 🔹 Step 1: Choose Launch Template

* **ASG name** → `my-auto-scaling-group`
* Select your **launch template**
* Version → **1 (Default)**

👉 Click **Next**

---

# 🔹 Step 2: Choose Instance Launch Options

## Network:

* **VPC** → Default VPC

* **Subnets (VERY IMPORTANT)**:

  * Select **at least 2 subnets**
  * Choose **different Availability Zones**

👉 Ensures high availability

---

# 🔹 Step 3: Integrate with Other Services

* Load Balancer options shown

👉 Select:

* **No Load Balancer**

👉 Click **Next**

---

# 🔹 Step 4: Configure Group Size and Scaling

## Capacity Settings:

* **Minimum capacity** → `1`
* **Desired capacity** → `2`
* **Maximum capacity** → `5`

---

## Scaling Policy:

* Select → **No scaling policy**

👉 Manual scaling for assignment

---

# 🔹 Step 5: Add Notifications

* Used for email/SNS alerts

👉 Skip (not required)

---

# 🔹 Step 6: Add Tags

* Click **Add tag**

Fill:

* **Key** → `Name`
* **Value** → `AutoScaling-Instance`

✅ Enable:

* **Propagate at launch**

👉 Ensures all instances get this name

---

# 🔹 Step 7: Review & Create

* Verify all settings carefully:

  * Template ✔️
  * Subnets ✔️
  * Capacity ✔️
  * Tags ✔️

👉 Click **Create Auto Scaling Group**

✔ Output: ASG created successfully

---

# 🔍 3. Verify Auto Scaling (VERY IMPORTANT)

---

## ✅ 1. Automatic Instance Creation

* Go to **EC2 → Instances**

✔ You should see:

* **2 instances running**

👉 Created automatically by ASG

---

## ✅ 2. Instance States

* Initially: **Pending**
* Then: **Running**

👉 Wait 1–2 minutes if needed

---

## ✅ 3. Check Tags

* Select instance → check **Tags tab**

✔ Should show:

```
Name = AutoScaling-Instance
```

---

## ✅ 4. Test Scale-Out (Increase Instances)

* Go to **Auto Scaling Group**
* Click **Edit**

Change:

```bash
Desired capacity → 3 or 4
```

✔ Result:

* New EC2 instances launch automatically

---

## ✅ 5. Test Scale-In (Decrease Instances)

Change:

```bash
Desired capacity → 1
```

✔ Result:

* Extra instances terminate automatically

---

## ✅ 6. Test Auto-Healing (Fault Tolerance)

* Go to EC2 Instances
* Manually **terminate 1 instance**

✔ Result:

* ASG automatically launches a **new instance**

---

# 🌐 4. (Optional) Test Website on Auto Instances

👉 If your template had Apache installed:

* Copy **public IP of any instance**
* Open browser:

```
http://<public-ip>
```

✔ Website should load

---

# 🧠 KEY CONCEPTS (VERY IMPORTANT)

---

## ☁️ Launch Template

* Defines EC2 configuration (AMI, instance type, key, security group)

---

## ⚙️ Auto Scaling Group

* Maintains required number of instances automatically

---

## 🔢 Capacity Terms

* **Minimum** → lowest number of instances
* **Desired** → current running instances
* **Maximum** → upper limit

---

## 🌍 Multi-AZ

* Using multiple subnets across AZs
* Provides **high availability**

---

## 🔄 Auto-Healing

* Failed instance → automatically replaced

---

## 🏷️ Tags

* Used to name/identify instances
* **Propagate at launch** is critical

---

## 🔐 No Scaling Policy (in this assignment)

* Scaling is tested manually
* Real-world uses CPU-based policies

---

# 🎉 FINAL RESULT

Auto Scaling was successfully implemented.

---

## ✅ Result in Points:

* Launch Template created successfully
* Auto Scaling Group configured
* EC2 instances launched automatically
* Scale-out and scale-in verified
* Fault tolerance tested via instance replacement

---

# 🧾 SMALL BUT IMPORTANT DETAILS (DON’T MISS)

* Always select **multiple subnets (different AZs)**
* Always enable **Propagate at launch** for tags
* “Updating capacity” = ASG is working
* Instances may take **1–2 minutes** to start
* Public IP changes for new instances
* Terminated instances are **auto replaced**


