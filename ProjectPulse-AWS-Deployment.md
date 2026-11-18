 ProjectPulse: Deploying a Maven Spring Boot Project on AWS EC2

## 1. Overview

As part of my AWS Cloud Internship, I successfully deployed my major project **ProjectPulse** on an AWS EC2 instance.

The primary objective of this task was to understand how to deploy a Maven-based Spring Boot application on an AWS EC2 Ubuntu server, configure a MySQL database, run the backend as a Linux systemd service, and make the application accessible through Nginx.

Along with the Spring Boot backend, I also deployed the React frontend and configured Nginx as a reverse proxy to connect frontend requests with backend APIs.

## 2. Technologies Used

- AWS EC2
- Ubuntu Server 24.04 LTS
- Java JDK 21
- Apache Maven 3.8.7
- Spring Boot 3.3.4
- MySQL 8.0.46
- React.js
- Node.js 22
- npm 10
- Nginx 1.24
- Linux systemd
- Git and GitHub

## 3. AWS EC2 Instance Configuration

| Configuration | Details |
|---|---|
| Instance Name | ProjectPulse-Server |
| Instance Type | t3.small |
| Operating System | Ubuntu Server 24.04 LTS |
| Region | Asia Pacific (Mumbai) |
| Java Version | 21 |
| Maven Version | 3.8.7 |
| Database | MySQL 8.0.46 |
| Backend Port | 8080 |
| Frontend Port | 80 |

## 4. Deployment Architecture

The application follows this architecture:

```text
                 User Browser
                      |
                      |
                 HTTP Port 80
                      |
                      v
                  NGINX
                      |
           +----------+----------+
           |                     |
           v                     v
      React Frontend         /api Requests
       (Static Files)             |
                                  v
                           Spring Boot API
                              Port 8080
                                  |
                                  v
                             MySQL Server
                              Port 3306
```

Nginx serves the React production build and forwards API requests to the Spring Boot backend.

The Spring Boot application connects to the MySQL database running on the same EC2 instance.

## 5. EC2 Server Preparation

Connected to the Ubuntu EC2 instance using AWS EC2 Instance Connect.

Updated the package repositories:

```bash
sudo apt update
sudo apt upgrade -y
```

Installed Java:

```bash
sudo apt install openjdk-21-jdk -y
java -version
```

Installed Maven:

```bash
sudo apt install maven -y
mvn -version
```

Installed MySQL:

```bash
sudo apt install mysql-server -y
sudo systemctl enable mysql
sudo systemctl start mysql
```

Verified MySQL:

```bash
sudo systemctl status mysql
```

## 6. MySQL Database Configuration

Created a dedicated database for ProjectPulse.

```sql
CREATE DATABASE projectpulse;
```

Created a dedicated MySQL user and granted database privileges.

The application connects to MySQL using environment variables configured on the EC2 server.

Verified database connectivity and confirmed that the application database was accessible.

## 7. Maven Spring Boot Application Build

Transferred the ProjectPulse source code to the EC2 instance.

Navigated to the backend directory:

```bash
cd /home/ubuntu/ProjectPulse/backend
```

Built the Spring Boot application using Maven:

```bash
mvn clean package -DskipTests
```

The Maven build completed successfully and generated the executable JAR:

```text
target/projectpulse-backend-1.0.0.jar
```

## 8. Spring Boot Backend Deployment

Configured a Linux systemd service to run the Spring Boot application in the background.

Created the service file:

```bash
sudo nano /etc/systemd/system/projectpulse.service
```

Service configuration:

```ini
[Unit]
Description=ProjectPulse Spring Boot Backend
After=network.target mysql.service
Requires=mysql.service

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/home/ubuntu/ProjectPulse/backend
EnvironmentFile=/etc/projectpulse/projectpulse.env
ExecStart=/usr/bin/java -jar /home/ubuntu/ProjectPulse/backend/target/projectpulse-backend-1.0.0.jar
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Reloaded systemd and enabled the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable projectpulse
sudo systemctl start projectpulse
```

Verified the backend service:

```bash
sudo systemctl status projectpulse
```

The Spring Boot backend successfully started on port 8080.

## 9. MySQL Migration Using Flyway

The application uses Flyway for database schema migration.

During Spring Boot startup, Flyway successfully validated the migration and confirmed that the database schema was up to date.

Verified the database tables:

```bash
mysql -u projectpulse_user -p -D projectpulse -e "SHOW TABLES;"
```

Successfully verified the application tables and `flyway_schema_history` table.

## 10. React Frontend Deployment

Installed Node.js and npm on the EC2 instance.

Navigated to the frontend directory:

```bash
cd /home/ubuntu/ProjectPulse/frontend
```

Installed dependencies:

```bash
npm ci
```

Created the production build:

```bash
npm run build
```

The production build was generated inside:

```text
/home/ubuntu/ProjectPulse/frontend/dist
```

## 11. Nginx Configuration

Installed and started Nginx:

```bash
sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
```

Configured Nginx to serve the React production build and forward backend API requests to Spring Boot.

Configuration file:

```text
/etc/nginx/sites-available/default
```

Nginx configuration:

```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name _;

    root /home/ubuntu/ProjectPulse/frontend/dist;
    index index.html;

    location /api/ {
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

Tested the Nginx configuration:

```bash
sudo nginx -t
```

Reloaded Nginx:

```bash
sudo systemctl reload nginx
```

Resolved frontend directory traversal permissions so that the Nginx worker could access the production files.

## 12. Application Verification

Successfully accessed the deployed application using the EC2 public IPv4 address over HTTP.

Verified the following:

- React frontend successfully loaded.
- Spring Boot backend was running as a systemd service.
- Nginx served the frontend successfully.
- Nginx reverse proxy forwarded API requests to Spring Boot.
- MySQL database connection was successful.
- Flyway migrations were applied successfully.
- User registration and login worked.
- Dashboard loaded backend data.
- Project creation and project member listing worked.
- Collaboration chat successfully sent and displayed messages.
- Chat message remained visible after refreshing the page.

### Backend API Health Check

Executed:

```bash
curl -i http://localhost/api/health
```

Received HTTP 200 OK.

API response:

```json
{
  "success": true,
  "message": "Operation successful",
  "data": {
    "version": "1.0.0",
    "service": "ProjectPulse API",
    "status": "UP"
  }
}
```

### Database Verification

Verified all application tables using MySQL.

The database contained 12 application tables along with the Flyway schema history table.

## 13. Deployment URL

Application URL at the time of deployment:

```text
http://13.204.76.107
```

Note: This is the EC2 public IP at the time of deployment. It may change if the instance is stopped and restarted without an Elastic IP.

## 14. Learning Outcomes

Through this task, I gained practical experience in:

1. Launching and configuring an AWS EC2 Ubuntu instance.
2. Installing Java JDK and Maven on Linux.
3. Building a Maven-based Spring Boot application.
4. Deploying an executable Spring Boot JAR on EC2.
5. Running Spring Boot as a background Linux systemd service.
6. Configuring MySQL on an EC2 instance.
7. Managing database migrations using Flyway.
8. Building and deploying a React frontend.
9. Configuring Nginx as a web server and reverse proxy.
10. Connecting frontend, backend, and database components.
11. Verifying application health and testing deployed features.

## 15. Task Status

**Status: Successfully Completed**

The ProjectPulse application was successfully deployed on AWS EC2. The React frontend, Spring Boot backend, Nginx reverse proxy, and MySQL database were configured and verified.

Note: The application was accessed over HTTP. Custom domain and HTTPS/SSL configuration were not included in this deployment task. The Desktop Agent and Hyperledger Fabric network were also not deployed as part of this task.
