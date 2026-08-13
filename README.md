# MERN Stack Application Deployment on Amazon EC2

**Author:** Adeniyi Abdulazeez (`hayzeddev123`)  
**Date:** August 2026  
**Instance:** `i-0e84b7653e6ab3e86` (MERN-server)  
**Public IP:** `13.48.129.163`  
**Region:** eu-north-1 (Europe - Stockholm)

---

## 1. Introduction

This project involved the deployment of a MERN (MongoDB, Express.js, React, Node.js) stack application on an Amazon Web Services (AWS) EC2 instance. The purpose of the project was to gain practical experience in cloud computing, Linux server administration, web application deployment, database configuration, networking, and reverse-proxy configuration.

The application was deployed on an Ubuntu-based Amazon EC2 instance. MongoDB Atlas was used as the cloud database, Node.js and Express.js were used for the backend, React was used for the frontend, Nginx was configured as the web server and reverse proxy, and PM2 was used to keep the Node.js backend running in the background.

---

## 2. Project Objectives

The objectives of the project were to:

- Create and configure an Amazon EC2 instance.
- Connect securely to the EC2 instance using SSH.
- Install and configure Git and Node.js.
- Set up a MERN application environment.
- Connect the Node.js backend to MongoDB Atlas.
- Deploy the React frontend.
- Configure Nginx to serve the React production build.
- Configure the EC2 security group and required network ports.
- Use PM2 to manage the Node.js backend process.
- Test the deployed application through the EC2 public IP address.
- Document the deployment process, commands, challenges, and solutions.

---

## 3. Technologies Used

| Technology       | Purpose                                      |
|------------------|----------------------------------------------|
| Amazon EC2       | Cloud server for hosting the application     |
| Ubuntu           | Operating system running on EC2              |
| Node.js          | JavaScript runtime for the backend           |
| Express.js       | Backend web framework                        |
| React + Vite     | Frontend user interface                      |
| MongoDB Atlas    | Cloud database                               |
| Nginx            | Web server and reverse proxy                 |
| PM2              | Node.js process manager                      |
| Git              | Source code management                       |
| SSH              | Secure remote access                         |

---

## 4. EC2 Instance Configuration

An Amazon EC2 instance was created to host the MERN application.

**Instance Details:**
- **Name:** MERN-server
- **Instance ID:** `i-0e84b7653e6ab3e86`
- **Type:** t3.small
- **State:** Running
- **Status Checks:** 3/3 checks passed
- **Public IPv4:** `13.48.129.163`
- **Private IP:** `172.31.9.164`
- **Availability Zone:** eu-north-1c
- **OS:** Ubuntu 26.04 LTS

The instance was accessed remotely using SSH with the private key (`MERN-key.pem`).

**Example SSH command:**
```bash
ssh -i "MERN-key.pem" ubuntu@13.48.129.163
```

Successful connection output:
```
Welcome to Ubuntu 26.04 LTS (GNU/Linux 7.0.0-1006-aws x86_64)
... (system information)
```

---

## 5. Installation of Required Software

After connecting to the Ubuntu EC2 instance, the required development tools were installed and verified.

```bash
node --version
npm --version
git --version
```

These tools provided the environment required to run and build the MERN application.

---

## 6. Project Structure

The application was organized into separate frontend and backend directories:

```
mern-app/
├── backend/
│   ├── server.js
│   ├── .env
│   ├── package.json
│   └── node_modules/
│
└── frontend/
    ├── src/
    ├── dist/
    ├── package.json
    └── node_modules/
```

---

## 7. MongoDB Atlas Configuration

MongoDB Atlas was used as the database service.

A MongoDB database user was configured for the application. The MongoDB Atlas connection string was stored in the backend `.env` file:

```env
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/database
PORT=5000
```

The connection was successfully tested. Backend output:
```
MongoDB connected
Server running on port 5000
```

---

## 8. Backend Deployment

The Node.js backend was deployed inside `~/mern-app/backend`.

Key steps:
```bash
cd ~/mern-app/backend
npm init -y
npm install express mongoose cors dotenv
npm install --save-dev nodemon
# Create server.js and .env
node server.js
```

Successful output:
```
MongoDB connected
Server running on port 5000
```

---

## 9. React Frontend Deployment

The React frontend was created using Vite:

```bash
cd ~/mern-app
npm create vite@latest frontend -- --template react
cd frontend
npm install
npm run dev -- --host 0.0.0.0
```

The application ran on port 5173 (or 5174 if occupied).

---

## 10. Building the React Application

```bash
cd ~/mern-app/frontend
npm run build
```

This generated the optimized production files in the `dist/` directory.

---

## 11. Nginx Installation and Configuration

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl status nginx
```

Nginx status: **Active: active (running)**

Custom configuration created at `/etc/nginx/sites-available/mern-app` to serve the React `dist` folder and reverse-proxy API requests.

```bash
sudo ln -s /etc/nginx/sites-available/mern-app /etc/nginx/sites-enabled/mern-app
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl restart nginx
```

Nginx test result: `syntax is ok` / `test is successful`

---

## 12. Nginx Permission Issue & Fix

**Problem:** Internal Server Error / Permission denied when accessing files in `/home/ubuntu/...`

**Solution:**
```bash
sudo chmod 755 /home/ubuntu
sudo chmod 755 /home/ubuntu/mern-app
sudo chmod 755 /home/ubuntu/mern-app/frontend
sudo chmod -R 755 /home/ubuntu/mern-app/frontend/dist
sudo systemctl restart nginx
```

After fixing permissions, the React application loaded successfully via Nginx on port 80.

---

## 13. PM2 Process Management

```bash
sudo npm install -g pm2
cd ~/mern-app/backend
pm2 start server.js --name mern-backend
pm2 status
pm2 save
pm2 startup
```

PM2 status showed the process as **online**.

This ensures the backend continues running even after the SSH session is closed.

---

## 14. AWS Security Group Configuration

Relevant ports opened:

| Port | Purpose                          |
|------|----------------------------------|
| 22   | SSH                              |
| 80   | Nginx (final web application)    |
| 5000 | Backend testing                  |
| 5173 | React development server testing |

---

## 15. Application Testing

- **MongoDB:** `MongoDB connected`
- **Backend:** `Server running on port 5000` + successful `curl http://localhost:5000`
- **Frontend (dev):** Accessible via public IP on port 5173/5174
- **Nginx (production):** React app served successfully at `http://13.48.129.163`

**Architecture:**
```
User Browser
     │
     ▼
EC2 Public IP (13.48.129.163)
     │
     ▼
Nginx :80
     │
     ▼
React Frontend (dist/)
     │
     ▼
Node.js / Express (PM2)
     │
     ▼
MongoDB Atlas
```

---

## 16. Challenges Encountered and Solutions

### Challenge 1: MongoDB Authentication Failure
**Error:** `bad auth : authentication failed`  
**Solution:** Reset MongoDB Atlas user password and update the `.env` file. Result: `MongoDB connected`

### Challenge 2: React Port Conflict
Vite reported port 5173 in use and switched to 5174.  
**Solution:** Stopped the conflicting process or used the new port.

### Challenge 3: Frontend Not Accessible Externally
**Solution:** Opened the required port in the Security Group and started Vite with `--host 0.0.0.0`.

### Challenge 4: Nginx Permission Denied / Internal Server Error
**Solution:** Adjusted directory permissions (`chmod 755` on home, mern-app, frontend, and dist) and restarted Nginx.

---

## 17. Final Application URL

**http://13.48.129.163**

Deployment Status: **Successfully deployed and tested.**

---

## 18. Key Commands Used

```bash
# SSH
ssh -i "MERN-key.pem" ubuntu@13.48.129.163

# Backend
cd ~/mern-app/backend
npm install express mongoose cors dotenv
node server.js
# or with PM2
pm2 start server.js --name mern-backend
pm2 save
pm2 startup

# Frontend
cd ~/mern-app/frontend
npm install
npm run build

# Nginx
sudo apt install nginx -y
sudo nginx -t
sudo systemctl restart nginx

# Permissions fix
sudo chmod 755 /home/ubuntu /home/ubuntu/mern-app /home/ubuntu/mern-app/frontend
sudo chmod -R 755 /home/ubuntu/mern-app/frontend/dist
```

---

## 19. Conclusion

The MERN stack application was successfully deployed on an Amazon EC2 instance. The project provided practical experience in cloud server configuration, Linux administration, SSH, Node.js, Express.js, React, MongoDB Atlas, Nginx, PM2, and AWS security groups.

The main deployment challenges involved MongoDB authentication, React port configuration, external network access, and Nginx file permissions. These issues were investigated using server commands and logs and were successfully resolved.

The final deployment allowed the React frontend to be accessed through the EC2 public IP, while Nginx handled the web traffic and PM2 maintained the Node.js backend process. MongoDB Atlas provided the database service for the application.

**Deployment Status: Successfully deployed and tested.**

---

*This repository documents the complete critical thinking / practical deployment process for a MERN stack application on AWS EC2.*
