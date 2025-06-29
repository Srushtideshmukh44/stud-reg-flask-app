
# 🎓 Student Registration Flask App – CI/CD on AWS using Jenkins

A complete full-stack deployment of a Flask-based Student Registration Web Application using Jenkins CI/CD, hosted on **AWS EC2**, and connected to **AWS RDS MySQL** in a custom VPC.

---

## 🚀 Project Overview

This project demonstrates:

- Flask-based web application (Student Registration)
- CI/CD pipeline using Jenkins
- Deployment on EC2
- MySQL database hosted on AWS RDS
- Secure infrastructure inside custom VPC
- Gunicorn for production server

---

## 🧱 Architecture

**3-Tier Deployment Structure:**

1. **Frontend** – HTML + Flask Form  
2. **Backend** – Flask Python App  
3. **Database** – Amazon RDS (MySQL)

---

## ⚙️ Technologies Used

- Python, Flask, HTML
- MySQL, Gunicorn
- Jenkins (CI/CD), GitHub
- Amazon EC2, Amazon RDS, VPC
- Linux (Ubuntu), Bash, Virtualenv

---

## 📦 Setup Guide (Step-by-Step)

### 🔹 Step 1: VPC and Subnets

- Create Custom VPC: `10.0.0.0/16`
- Create Subnets:
  - Public: `10.0.1.0/24`
  - Private: `10.0.2.0/24`
- Attach Internet Gateway to VPC
- Create Public Route Table → Add 0.0.0.0/0 via IGW → Associate with Public Subnet

---

### 🔹 Step 2: RDS MySQL Setup

- Launch RDS:
  - Engine: MySQL
  - DB Name: `studentdb`
  - Username: `admin`
  - Password: `Srushti123`
  - Public Access: Yes (for testing)
- Create RDS Security Group:
  - Type: MySQL/Aurora
  - Port: 3306
  - Source: EC2 Security Group
- Create Table:

```sql
CREATE TABLE students (
  name VARCHAR(100),
  email VARCHAR(100),
  phone VARCHAR(20),
  course VARCHAR(50),
  address VARCHAR(255)
);
```

---

### 🔹 Step 3: EC2 + Flask Setup

- Launch EC2 (Ubuntu 22.04) in Public Subnet with:
  - Port 22 (SSH)
  - Port 5000 (Flask)
  - Port 8080 (Jenkins)
- SSH & install dependencies:

```bash
sudo apt update -y
sudo apt install python3-pip python3-venv git -y
```

- Clone project & setup Flask:

```bash
git clone https://github.com/srushtideshmukh44/stud-reg-flask-app.git
cd stud-reg-flask-app
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

- Edit `app.py` for public access:

```python
app.run(host='0.0.0.0', port=5000, debug=True)
```

- Set correct `db_config` using RDS credentials.

---

### 🔹 Step 4: Jenkins Installation on EC2

```bash
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update -y
sudo apt install openjdk-17-jdk jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

- Visit `http://<EC2-Public-IP>:8080`  
- Unlock Jenkins using:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

### 🔹 Step 5: Jenkins CI/CD Pipeline

- Create Freestyle Job: `student-registration-deploy`
- GitHub repo: https://github.com/srushtideshmukh44/stud-reg-flask-app.git
- Shell Script (Execute Shell Step):

```bash
rm -rf stud-reg-flask-app
git clone https://github.com/srushtideshmukh44/stud-reg-flask-app.git
cd stud-reg-flask-app
python3 -m venv venv
. venv/bin/activate
pip install -r requirements.txt
gunicorn -b 0.0.0.0:5000 app:app --daemon
```

---

### 🔹 Step 6: Test the Application

- Jenkins Console: ✅ Build should show SUCCESS  
- Verify `gunicorn` process is running:

```bash
ps aux | grep gunicorn
```

- Visit the app in browser:

```
http://<your-ec2-ip>:5000
```

- Fill the registration form and test RDS insert

---

## ✅ Final Checklist

| Component            | Status |
|----------------------|--------|
| VPC & Subnet         | ✅ Done |
| EC2 Setup            | ✅ Done |
| RDS MySQL            | ✅ Done |
| Flask App            | ✅ Done |
| Jenkins CI/CD        | ✅ Done |
| Gunicorn Deployment  | ✅ Done |
| End-to-End Testing   | ✅ Done |

---

## 🙋‍♀️ Author

**Srushti Deshmukh**  
[Linkdin Profile]:www.linkedin.com/in/srushti-deshmukh44
