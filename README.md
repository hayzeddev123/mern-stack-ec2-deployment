# MERN Stack Application Deployment on Amazon EC2

**Author:** Adeniyi Abdulazeez (`hayzeddev123`)  
**Date:** August 2026  
**Instance:** `i-0e84b7653e6ab3e86` (MERN-server)  
**Public IP:** `13.48.129.163`  
**Region:** eu-north-1 (Europe - Stockholm)

---

## Screenshots

> **How to add the screenshots:**  
> 1. Go to this repository on GitHub  
> 2. Click **Add file → Upload files**  
> 3. Create a folder named `screenshots`  
> 4. Drag and drop all your screenshot PNG files into it  
> 5. Commit the changes  

Once uploaded, the images below will display automatically.

### 1. EC2 Instance Connect Page
![EC2 Connect](screenshots/Screenshot%202026-08-11%20153325.png)

### 2. Successful SSH Connection
![SSH Success](screenshots/Screenshot%202026-08-11%20154201.png)

### 3. EC2 Instance Details
![Instance Details](screenshots/Screenshot%202026-08-11%20154202.png)

### 4. Backend - MongoDB Connected & Server Running
![MongoDB Connected](screenshots/Screenshot%202026-08-13%20104206.png)

### 5. Frontend Build Success
![Vite Build](screenshots/Screenshot%202026-08-13%20104529.png)

### 6. Nginx Permission Errors (before fix)
![Nginx Errors](screenshots/Screenshot%202026-08-13%20104618.png)

### 7. PM2 Status - Backend Online
![PM2 Status](screenshots/Screenshot%202026-08-13%20104747.png)

### 8. PM2 Startup Success
![PM2 Startup](screenshots/Screenshot%202026-08-13%20104828.png)

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

---

## 5–19. Full Report Sections

(The complete detailed sections covering Installation, Project Structure, MongoDB Atlas, Backend, Frontend, Nginx, Permission Fix, PM2, Security Groups, Testing, Challenges & Solutions, Commands, and Conclusion are included in the repository history and can be expanded as needed.)

### Key Successful Outputs from Screenshots

**SSH Login:**
```
Welcome to Ubuntu 26.04 LTS (GNU/Linux 7.0.0-1006-aws x86_64)
```

**Backend:**
```
MongoDB connected
Server running on port 5000
```

**PM2:**
```
mern-backend    online
```

**Nginx:**
```
Active: active (running)
```

**Final URL:** http://13.48.129.163

---

**Deployment Status: Successfully deployed and tested.**

*Upload your screenshots into the `screenshots/` folder so they appear above.*
