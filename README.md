#  EC2 + Docker + GitHub Secrets Setup Guide

This document explains how to:

* Create an EC2 instance
* Install Docker on Ubuntu
* Verify Docker access for the ubuntu user
* Create required GitHub Secrets for CI/CD

---

## 1 Create an EC2 Instance (Ubuntu)

### Steps

1. Log in to AWS Console
2. Navigate to **EC2 → Launch Instance**
3. Choose:

   * **AMI**: Ubuntu Server 22.04 LTS
   * **Instance Type**: t2.micro (Free Tier eligible)
4. Create or select an existing **Key Pair (.pem file)**
5. Configure Security Group:

   * Allow **SSH (port 22)** from your IP
6. Launch the instance

---

## 2 Connect to the EC2 Instance

```bash
ssh -i your-key.pem ubuntu@<EC2_PUBLIC_IP>
```

---

## 3 Install Docker on Ubuntu

Run the following commands in order:

```bash
sudo su
apt update
curl https://get.docker.com | bash
usermod -aG docker ubuntu
```

 **Important**
After running the `usermod` command:

* Close the terminal completely
* Open a new terminal session

Reconnect:

```bash
ssh -i your-key.pem ubuntu@<EC2_PUBLIC_IP>
```

---

##  Verify Docker Access (Without sudo)

Run:

```bash
docker --version
docker ps
```

Optional test:

```bash
docker run hello-world
```

 Docker should work without `sudo`.

---

##  Create GitHub Secrets

Navigate to:

```
GitHub Repository → Settings → Secrets and variables → Actions
```

Click **New repository secret** for each secret below.

---

##  Required GitHub Secrets

### 1. DOCKER_USERNAME

Docker Hub username

Example:

```
powerstar123
```

---

### 2. DOCKER_PASSWORD

Docker Hub **Access Token**

Create it from:

```
Docker Hub → Account Settings → Security → New Access Token
```

---

### 3. EC2_HOST

Public IP address of EC2 instance

Example:

```
13.235.xxx.xxx
```

---

### 4. EC2_USER

Default EC2 user

```
ubuntu
```

---

### 5. EC2_SSH_KEY

Contents of your `.pem` file (entire file)

Example:

```
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEA...
...
-----END RSA PRIVATE KEY-----
```

 Do not modify or trim spaces/new lines.

---

## 6 Verification Checklist

| Step                      | Status |
| ------------------------- | ------ |
| EC2 Instance Created      | ✅      |
| Docker Installed          | ✅      |
| Docker works without sudo | ✅      |
| GitHub Secrets Added      | ✅      |

---

## 7 Common Issues & Fixes

### Docker permission denied

Fix:

```bash
groups
```

Ensure `docker` group is listed.

If not:

```bash
sudo usermod -aG docker ubuntu
```

Log out and log back in.

---

##  Next Steps

* Create GitHub Actions pipeline
* Build Docker image
* Push image to Docker Hub
* Deploy container on EC2 via SSH

This setup is now ready for CI/CD automation 
