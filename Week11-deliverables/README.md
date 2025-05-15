
# MERN Stack Blog App Deployment with Terraform and Ansible

## Infrastructure Bootcamp - Week 11 Assignment  
**Author:** Roaa Saeedi  
**Date:** May 2025

---

## Project Overview

This project demonstrates the deployment of a production-ready MERN stack blog application using modern DevOps tools.  
It includes Infrastructure as Code (IaC) using Terraform, configuration management with Ansible, and integration with AWS and MongoDB Atlas.

---

## Architecture Diagram

![Architecture Diagram](A_2D_digital_diagram_illustrates_the_architecture_.png)

**Components:**

- **EC2**: Hosts the backend Node.js app with PM2.
- **MongoDB Atlas**: Cloud-hosted NoSQL database.
- **S3 (Frontend Bucket)**: Hosts React static frontend.
- **S3 (Media Bucket)**: Stores uploaded media with proper IAM permissions.

---

## 1. Security Configuration

- **IAM User for S3 Media Access**
  - Created via Terraform with custom IAM policy.
  - Credentials retrieved using:
    ```bash
    terraform output -raw s3_user_access_key  
    terraform output -raw s3_user_secret_key
    ```

- **Security Group**
  - Allowed inbound traffic on:
    - SSH (22)
    - HTTP (80)
    - App Port (5000)

---

## 2. MongoDB Atlas Setup

1. Created a free-tier MongoDB cluster.
2. Whitelisted EC2 public IP address.
3. Created database user with proper privileges.
4. Retrieved MongoDB connection string and injected into backend `.env` file.

> Screenshot: `mongodb-cluster.png`

---

## 3. S3 Buckets Configuration

### Frontend Bucket
- Enabled **static website hosting**.
- Applied **public read policy** to allow web access.

### Media Bucket
- Configured with:
  - CORS policy for file uploads.
  - IAM access only for backend (no public access).

> Screenshot: `s3-frontend.png`, `s3-media-upload.png`

---

## 4. Backend Deployment with Terraform + Ansible

### EC2 Launch Template
- Ubuntu 22.04
- t3.micro instance
- Key pair for SSH

### Ansible Backend Provisioning
- Role-based structure:
  ```
  ansible/
  ├── inventory
  ├── backend-playbook.yml
  └── roles/
      └── backend/
          ├── tasks/
          ├── templates/
          └── vars/
  ```

### Tasks Performed:
- Cloned MERN app from GitHub.
- Created `.env` file dynamically using Ansible template.
- Installed dependencies with `npm install`.
- Started backend with **PM2** and enabled auto-restart.

> Screenshot: `pm2-status.png`

---

## 5. Frontend Deployment to S3

- Configured `.env` for frontend with:
  ```env
  VITE_BASE_URL=http://<ec2-dns>:5000/api
  VITE_MEDIA_BASE_URL=https://<media-bucket>.s3.eu-north-1.amazonaws.com
  ```
- Used AWS CLI to deploy the frontend:
  ```bash
  pnpm run build
  aws s3 sync dist/ s3://<frontend-bucket-name>/
  ```

> Screenshot: `frontend-in-browser.png`

---

## 6. Success Criteria

- [x] Backend connected to MongoDB Atlas  
- [x] Frontend accessible via S3 static hosting  
- [x] Media uploads working through S3  
- [x] Backend auto-starts with PM2  

---

## 7. Cleanup Instructions

- Run `terraform destroy` to delete all resources.
- Remove `.env` and credentials from EC2.
- Delete MongoDB users and IP access entries.
- Revoke any IAM users/keys manually created.

---

## Tools Used

- Terraform
- Ansible
- AWS (EC2, S3, IAM)
- MongoDB Atlas
- PM2
- Node.js / React / Vite

---

## Screenshots

- `architecture-diagram.png`
- `pm2-status.png`
- `mongodb-cluster.png`
- `s3-frontend.png`
- `s3-media-upload.png`
- `frontend-in-browser.png`
