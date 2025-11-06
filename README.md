# 🚀 Automated Python Portfolio Website Deployment on AWS EC2 using Jenkins CI/CD

This project demonstrates a fully automated deployment pipeline for a **Python-based portfolio website** hosted on **AWS EC2**, integrated with **Jenkins** and **GitHub Webhooks** for continuous integration and delivery (CI/CD).

---

## 🧩 Project Overview

The goal of this project is to automate the end-to-end deployment process of a personal portfolio website. Every time new code is pushed to the GitHub repository, Jenkins automatically triggers the build, deploys the latest version on the EC2 instance, and serves it live to the users.

---

## ⚙️ Tech Stack

| Component           | Technology Used           |
| ------------------- | ------------------------- |
| **Frontend**        | HTML, CSS, JavaScript     |
| **Backend**         | Python (Flask/Django)     |
| **Server**          | AWS EC2 (Ubuntu)          |
| **Automation Tool** | Jenkins                   |
| **Version Control** | Git & GitHub              |
| **Web Server**      | Nginx / Apache (optional) |
| **Triggering**      | GitHub Webhook            |

---

## 🏗️ Architecture

```text
       ┌─────────────┐        ┌─────────────┐        ┌───────────────┐
       │   Developer │  Push  │   GitHub    │  Hook  │   Jenkins CI   │
       └─────────────┘ ─────▶ └─────────────┘ ─────▶ └───────────────┘
                                                    │  Pull latest   │
                                                    │  Build & Test  │
                                                    │  Deploy to EC2 │
                                                    └───────┬────────┘
                                                            │
                                                            ▼
                                                   ┌───────────────────┐
                                                   │ AWS EC2 (Python)  │
                                                   │ Live Portfolio App│
                                                   └───────────────────┘
```

🖼️ **Architecture Diagram:**
![Architecture Diagram](screenshots/architecture.png)

---

## 🔧 Setup & Configuration

### 1️⃣ Launch AWS EC2 Instance

* Launch an **Ubuntu EC2 instance**
* SSH into the instance:

  ```bash
  ssh -i "key.pem" ubuntu@<EC2-Public-IP>
  ```
* Update system and install dependencies:

  ```bash
  sudo apt update -y
  sudo apt install python3 python3-pip git -y
  ```

🖼️ **EC2 Setup Screenshot:**
![EC2 Instance Launch](screenshots/ec2-launch.png)

---

### 2️⃣ Install & Configure Jenkins

* Install Jenkins on EC2:

  ```bash
  sudo apt install openjdk-17-jdk -y
  wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
  sudo sh -c 'echo deb https://pkg.jenkins.io/debian/ binary/ > /etc/apt/sources.list.d/jenkins.list'
  sudo apt update
  sudo apt install jenkins -y
  ```
* Start Jenkins:

  ```bash
  sudo systemctl start jenkins
  sudo systemctl enable jenkins
  ```
* Access Jenkins from browser:
  `http://<EC2-Public-IP>:8080`

🖼️ **Jenkins Dashboard Screenshot:**
![Jenkins Dashboard](screenshots/jenkins-dashboard.png)

---

### 3️⃣ Jenkins Configuration

* Install required plugins: **Git**, **Pipeline**, **GitHub Integration**
* Create a **New Pipeline Job**

  * In **Source Code Management**, select **Git**
  * Add your **GitHub repository URL**
  * Choose **Build Triggers → GitHub hook trigger for GITScm polling**
  * Add a **Jenkinsfile** in your GitHub repository (sample below)

🖼️ **Jenkins Job Configuration Screenshot:**
![Jenkins Job Configuration](screenshots/jenkins-job.png)

---

### 4️⃣ Jenkinsfile Example

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/<your-username>/<your-repo>.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip3 install -r requirements.txt'
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                pkill -f app.py || true
                nohup python3 app.py > app.log 2>&1 &
                '''
            }
        }
    }
}
```

🖼️ **Jenkins Build Console Screenshot:**
![Build Console](screenshots/jenkins-build.png)

---

### 5️⃣ Configure GitHub Webhook

* Go to your **GitHub repository → Settings → Webhooks**
* Add a new webhook:

  ```
  Payload URL: http://<EC2-Public-IP>:8080/github-webhook/
  Content type: application/json
  Trigger: Just the push event
  ```
* Now every time you push code → Jenkins automatically deploys the updated site 🎉

🖼️ **GitHub Webhook Setup Screenshot:**
![GitHub Webhook](screenshots/github-webhook.png)

---

## 🧠 CI/CD Workflow

| Step | Description                               |
| ---- | ----------------------------------------- |
| 1️⃣  | Developer commits & pushes code to GitHub |
| 2️⃣  | Webhook triggers Jenkins build            |
| 3️⃣  | Jenkins pulls latest code                 |
| 4️⃣  | Dependencies installed, tests run         |
| 5️⃣  | Jenkins deploys new build to EC2          |
| 6️⃣  | Portfolio site updates automatically      |

🖼️ **Pipeline Run Screenshot:**
![Pipeline Run](screenshots/pipeline-run.png)

---

## 🖥️ Verification

Visit your **EC2 Public IP** or domain (if mapped via Route53):

```
http://<EC2-Public-IP>:5000
```

🖼️ **Live Website Screenshot:**
![Live Portfolio Website](screenshots/live-site.png)

---

## 🧾 Author

**Vivek Chopade**
📎 [GitHub](https://github.com/chopadevivek07)
💼 [LinkedIn](https://www.linkedin.com/in/vivek-chopade-4466)

---

## 🏁 Conclusion

This project showcases a **fully automated deployment pipeline** using **Jenkins CI/CD** integrated with **GitHub Webhook**, enabling continuous delivery of a **Python portfolio website** hosted on **AWS EC2**.
Every code change instantly reflects on the live website — achieving a seamless DevOps workflow! ✨

---
