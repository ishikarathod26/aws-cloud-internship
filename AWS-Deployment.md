# Library Management System – AWS Deployment

## 1. Project Overview

Successfully deployed a Spring Boot-based Library Management System on AWS EC2 with MySQL database migration and REST API verification.

The application manages books, students, and book issue records.

## 2. Technologies Used

- Java 25
- Spring Boot
- Maven
- MySQL 8.0
- AWS EC2
- Ubuntu Server 24.04 LTS
- GitHub
- Swagger UI
- systemd

## 3. AWS Infrastructure

| Configuration | Details |
|---|---|
| Cloud Provider | AWS |
| Service | EC2 |
| Instance Name | library-management-server |
| Instance Type | t3.micro |
| Operating System | Ubuntu 24.04 LTS |
| Region | Mumbai (ap-south-1) |
| Availability Zone | ap-south-1b |
| Storage | 20 GiB gp3 |
| Application Port | 9090 |
| Database | MySQL 8.0 |

## 4. Deployment Steps

### Step 1: Launch EC2 Instance

- Launched an Ubuntu 24.04 LTS EC2 instance.
- Configured security group rules.
- Allowed SSH traffic on port 22.
- Allowed application traffic on port 9090.
- Connected to EC2 using SSH and a PEM key pair.

### Step 2: Install Java

```bash
sudo apt update
sudo apt install openjdk-25-jre-headless -y
java -version
```

### Step 3: Install MySQL

```bash
sudo apt install mysql-server -y
sudo systemctl status mysql
```

Created the application database:

```sql
CREATE DATABASE library_db;
```

Created a separate MySQL application user:

```sql
CREATE USER 'library_app'@'localhost' IDENTIFIED BY 'YOUR_PASSWORD';

GRANT ALL PRIVILEGES ON library_db.* TO 'library_app'@'localhost';

FLUSH PRIVILEGES;
```

### Step 4: Migrate Existing Database

Exported the local MySQL database:

```bash
mysqldump -u root -p --databases library_db --routines --triggers > library_db_backup.sql
```

Transferred the backup to EC2 using SCP:

```bash
scp -i "library-server-key.pem" library_db_backup.sql ubuntu@13.203.102.220:/home/ubuntu/
```

Imported the database:

```bash
sudo mysql < /home/ubuntu/library_db_backup.sql
```

Verified the migrated tables:

```sql
USE library_db;
SHOW TABLES;
```

Successfully migrated:
- book
- students
- book_issues

### Step 5: Build Spring Boot Application

Built the executable JAR locally:

```cmd
mvnw.cmd clean package
```

Build completed successfully with all 48 tests passing.

### Step 6: Upload JAR to EC2

```bash
scp -i "library-server-key.pem" library-management-system-0.0.1-SNAPSHOT.jar ubuntu@13.203.102.220:/home/ubuntu/
```

### Step 7: Configure Environment Variables

Created an environment file:

```bash
sudo nano /home/ubuntu/library-management.env
```

Configured the following variables:

```ini
SPRING_DATASOURCE_URL=jdbc:mysql://localhost:3306/library_db?createDatabaseIfNotExist=true&useSSL=false&allowPublicKeyRetrieval=true
SPRING_DATASOURCE_USERNAME=library_app
SPRING_DATASOURCE_PASSWORD=YOUR_PASSWORD
```

Secured the environment file:

```bash
sudo chmod 600 /home/ubuntu/library-management.env
```

### Step 8: Configure systemd Service

Created the service file:

```bash
sudo nano /etc/systemd/system/library-management.service
```

Service configuration:

```ini
[Unit]
Description=Library Management Spring Boot Application
After=network.target mysql.service
Requires=mysql.service

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu
EnvironmentFile=/home/ubuntu/library-management.env
ExecStart=/usr/bin/java -jar /home/ubuntu/library-management-system-0.0.1-SNAPSHOT.jar
SuccessExitStatus=143
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Reloaded systemd and enabled the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable library-management
sudo systemctl start library-management
```

Verified the service:

```bash
sudo systemctl status library-management
```

Status: `active (running)`

### Step 9: Verify Application

Application URL:

http://13.203.102.220:9090

Swagger UI:

http://13.203.102.220:9090/swagger-ui/index.html

Tested the following APIs:

| Endpoint | Method | Response |
|---|---|---|
| `/books` | GET | 200 OK |
| `/students` | GET | 200 OK |
| `/book-issues` | GET | 200 OK |
| `/book-issues/active` | GET | 200 OK |

Existing database records were successfully retrieved from the deployed application.

### Step 10: Verify Background Execution

- Disconnected the SSH session.
- Refreshed Swagger UI.
- Executed the GET `/books` endpoint again.
- Received `200 OK`.

This confirmed that systemd keeps the Spring Boot application running independently of the SSH session.

## 5. Important Commands and Their Purpose

| Command | Purpose |
|---|---|
| `java -version` | Verify Java installation |
| `sudo apt update` | Update package information |
| `sudo systemctl status mysql` | Check MySQL status |
| `mysqldump` | Export database backup |
| `scp` | Transfer files securely |
| `sudo mysql < backup.sql` | Import database |
| `java -jar` | Execute Spring Boot JAR |
| `systemctl start` | Start application service |
| `systemctl enable` | Enable service at startup |
| `systemctl status` | Check service status |

## 6. Security Considerations

- SSH access restricted using the security group's source IP rule.
- MySQL configured for local application access.
- Database credentials stored in an environment file.
- Environment file permissions restricted using `chmod 600`.
- Database credentials and SQL backup excluded from GitHub.
- EC2 instance should be stopped when not required to avoid unnecessary compute charges. EBS storage charges may continue while stopped.

## 7. Final Outcome

Successfully deployed the Library Management System on AWS EC2.

Key achievements:

- EC2 instance provisioning and configuration.
- Java and MySQL installation.
- Existing database migration from local MySQL to AWS.
- Spring Boot executable JAR deployment.
- systemd service configuration.
- REST API verification using Swagger UI.
- Background execution verification after SSH disconnection.

**Deployment Status: Successfully Completed.**
