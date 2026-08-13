# MERN Stack Application Deployment on Amazon EC2

**Author:** Adeniyi Abdulazeez (`hayzeddev123`)  
**Date:** August 2026  
**Instance ID:** `i-0e84b7653e6ab3e86` (MERN-server)  
**Public IP:** `13.48.129.163`  
**Region:** eu-north-1 (Europe - Stockholm)  
**OS:** Ubuntu 26.04 LTS

---

## 1. Introduction

This project involved the deployment of a MERN (MongoDB, Express.js, React, Node.js) stack application on an Amazon Web Services (AWS) EC2 instance. The purpose of the project was to gain practical experience in cloud computing, Linux server administration, web application deployment, database configuration, networking, and reverse-proxy configuration.

The application was deployed on an Ubuntu-based Amazon EC2 instance. MongoDB Atlas was used as the cloud database, Node.js and Express.js were used for the backend, React (with Vite) was used for the frontend, Nginx was configured as the web server and reverse proxy, and PM2 was used to keep the Node.js backend running in the background.

---

## 2. Project Objectives

- Create and configure an Amazon EC2 instance
- Connect securely to the EC2 instance using SSH
- Install and configure Git and Node.js
- Set up a MERN application environment
- Connect the Node.js backend to MongoDB Atlas
- Deploy the React frontend
- Configure Nginx to serve the React production build
- Configure the EC2 security group and required network ports
- Use PM2 to manage the Node.js backend process
- Test the deployed application through the EC2 public IP address
- Document the deployment process, commands, challenges, and solutions

---

## 3. Technologies Used

| Technology       | Purpose                                      |
|------------------|----------------------------------------------|
| Amazon EC2       | Cloud server for hosting the application     |
| Ubuntu 26.04 LTS | Operating system running on EC2              |
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

- **Name:** MERN-server
- **Instance ID:** `i-0e84b7653e6ab3e86`
- **Instance Type:** t3.small
- **State:** Running
- **Status Checks:** 3/3 checks passed
- **Public IPv4:** `13.48.129.163`
- **Private IP:** `172.31.9.164`
- **Availability Zone:** eu-north-1c

**EC2 Connect page:**

![EC2 Connect](Screenshot%202026-08-11%20153325.png)

**Instance details in AWS Console:**

![Instance Details](Screenshot%202026-08-11%20154202.png)

SSH connection command used:
```bash
ssh -i "MERN-key.pem" ubuntu@13.48.129.163
```

**Successful SSH connection:**

![SSH Success](Screenshot%202026-08-11%20154201.png)

---

## 5. Installation of Required Software

```bash
sudo apt update
node --version
npm --version
git --version
```

---

## 6. Project Structure

```
mern-app/
├── backend/
│   ├── server.js
│   ├── .env
│   ├── package.json
│   └── node_modules/
└── frontend/
    ├── src/
    ├── dist/
    ├── package.json
    └── node_modules/
```

---

## 7. MongoDB Atlas Configuration

The MongoDB Atlas connection string was stored securely in the `.env` file:

```env
MONGO_URI=mongodb+srv://...
PORT=5000
```

---

## 8. Backend Deployment

```bash
cd ~/mern-app/backend
npm init -y
npm install express mongoose cors dotenv
npm install --save-dev nodemon
node server.js
```

**Backend running successfully with MongoDB connected:**

![MongoDB Connected](Screenshot%202026-08-13%20104206.png)

---

## 9. React Frontend Deployment

```bash
cd ~/mern-app
npm create vite@latest frontend -- --template react
cd frontend
npm install
npm run dev -- --host 0.0.0.0
```

---

## 10. Building the React Application

```bash
cd ~/mern-app/frontend
npm run build
```

This created the optimized production files in the `dist/` folder.

**Successful production build:**

![Vite Build](Screenshot%202026-08-13%20104529.png)

---

## 11. Nginx Installation and Configuration

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl status nginx
```

Nginx was configured to serve the React `dist` folder and act as a reverse proxy.

```bash
sudo ln -s /etc/nginx/sites-available/mern-app /etc/nginx/sites-enabled/mern-app
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl restart nginx
```

---

## 12. Nginx Permission Issue & Fix

**Problem:** `Permission denied` errors when Nginx tried to access files under `/home/ubuntu/`.

**Nginx error logs showing permission issues:**

![Nginx Errors](Screenshot%202026-08-13%20104618.png)

**Solution:**
```bash
sudo chmod 755 /home/ubuntu
sudo chmod 755 /home/ubuntu/mern-app
sudo chmod 755 /home/ubuntu/mern-app/frontend
sudo chmod -R 755 /home/ubuntu/mern-app/frontend/dist
sudo systemctl restart nginx
```

After the fix, the application loaded correctly on port 80.

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

**PM2 showing the backend process online:**

![PM2 Status](Screenshot%202026-08-13%20104747.png)

**PM2 startup and save success:**

![PM2 Startup](Screenshot%202026-08-13%20104828.png)

The backend process will now restart automatically after a server reboot.

---

## 14. AWS Security Group Configuration

| Port | Purpose                          |
|------|----------------------------------|
| 22   | SSH                              |
| 80   | Nginx (final web application)    |
| 5000 | Backend testing                  |
| 5173 | React development server testing |

---

## 15. Application Testing

- MongoDB: `MongoDB connected`
- Backend: `Server running on port 5000`
- Frontend (dev): Accessible externally
- Production: React app served by Nginx at **http://13.48.129.163**

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
Node.js / Express (managed by PM2)
     │
     ▼
MongoDB Atlas
```

---

## 16. Challenges Encountered and Solutions

**1. MongoDB Authentication Failure**  
Error: `bad auth : authentication failed`  
Solution: Reset the MongoDB Atlas user password and updated the `.env` file.

**2. React Port Conflict**  
Vite switched from 5173 to 5174.  
Solution: Used the available port or stopped the conflicting process.

**3. Frontend Not Accessible Externally**  
Solution: Opened the required port in the Security Group and started Vite with `--host 0.0.0.0`.

**4. Nginx Permission Denied / Internal Server Error**  
Solution: Adjusted directory permissions with `chmod 755` and restarted Nginx (see screenshot in Section 12).

---

## 17. Final Application URL

**http://13.48.129.163**

**Deployment Status:** Successfully deployed and tested.

---

## 18. Key Commands Used

```bash
# SSH
ssh -i "MERN-key.pem" ubuntu@13.48.129.163

# Backend
cd ~/mern-app/backend
npm install express mongoose cors dotenv
node server.js
pm2 start server.js --name mern-backend
pm2 save && pm2 startup

# Frontend
cd ~/mern-app/frontend
npm install
npm run build

# Nginx
sudo apt install nginx -y
sudo nginx -t
sudo systemctl restart nginx

# Permission fix
sudo chmod 755 /home/ubuntu /home/ubuntu/mern-app /home/ubuntu/mern-app/frontend
sudo chmod -R 755 /home/ubuntu/mern-app/frontend/dist
```

---

## 19. Conclusion

The MERN stack application was successfully deployed on an Amazon EC2 instance. The project provided practical experience in cloud server configuration, Linux administration, SSH, Node.js, Express.js, React, MongoDB Atlas, Nginx, PM2, and AWS security groups.

The main challenges (MongoDB authentication, port conflicts, external access, and Nginx permissions) were investigated using logs and commands and successfully resolved.

The final deployment allows the React frontend to be accessed through the EC2 public IP, with Nginx handling web traffic and PM2 keeping the backend running continuously.

**Deployment Status: Successfully deployed and tested.**
