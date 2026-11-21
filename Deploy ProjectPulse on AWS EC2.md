 ProjectPulse AWS Deployment
1. Objective
The objective of this task was to deploy the ProjectPulse full-stack application on AWS EC2 and make it publicly accessible over the internet.
The deployment includes a React frontend, Spring Boot backend, MariaDB database, and Nginx web server configured as a reverse proxy.
2. Technologies and AWS Services Used
AWS Services
	• Amazon EC2: Hosted the ProjectPulse application.
	• IAM Role: Provided the EC2 instance with permissions to access deployment artifacts stored in Amazon S3.
	• Amazon S3: Used to store and retrieve deployment artifacts.
Technologies
	• Java and Spring Boot
	• React.js
	• MariaDB
	• Nginx
	• Linux (Amazon Linux 2023)
	• GitHub Actions
	• Maven
	• AWS Systems Manager Session Manager
3. Application Architecture
The application follows a three-layer deployment architecture:
	1. Frontend: React application served by Nginx.
	2. Backend: Spring Boot application running on port 8080.
	3. Database: MariaDB storing application data.
Nginx acts as a reverse proxy:
	• Routes frontend requests to the React application.
	• Forwards /api/ requests to the Spring Boot backend.
	• Keeps the backend port private instead of exposing port 8080 publicly.
4. Implementation Steps
Step 1: EC2 Instance Setup
	• Created an Amazon EC2 instance using Amazon Linux 2023.
	• Configured the required security group rules.
	• Enabled access through AWS Systems Manager Session Manager.
Step 2: Backend Deployment
	• Prepared the Spring Boot application for production deployment.
	• Built the application using Maven.
	• Deployed the generated JAR file on EC2.
	• Configured a systemd service named projectpulse.service.
	• Verified that the Spring Boot backend was running on port 8080.
Step 3: Database Configuration
	• Installed and configured MariaDB on the EC2 instance.
	• Created the projectpulse database.
	• Configured Spring Boot database connectivity.
	• Verified the database connection.
Step 4: Frontend Deployment
	• Generated the production build of the React application.
	• Deployed the frontend build files to /var/www/projectpulse.
	• Configured Nginx to serve the frontend application.
Step 5: Nginx Reverse Proxy Configuration
	• Configured Nginx to serve the frontend.
	• Added reverse proxy configuration for /api/ requests.
	• Forwarded API requests to http://127.0.0.1:8080.
	• Configured frontend routing using try_files.
	• Validated the Nginx configuration and reloaded the service.
Step 6: Deployment Verification
	• Accessed the application using the EC2 public IPv4 address.
	• Verified that the login and registration pages loaded.
	• Tested the backend API through the Nginx reverse proxy.
	• Verified login and registration functionality.
	• Confirmed frontend, backend, and database connectivity.
5. Security Configuration
	• Allowed HTTP traffic on port 80.
	• Restricted SSH access to the required source IP.
	• Kept the Spring Boot backend port 8080 inaccessible from the public internet.
	• Used an EC2 IAM role for AWS service access.
	• Used Nginx as the public entry point for frontend and API requests.
6. Final Outcome
Successfully deployed the ProjectPulse application on AWS EC2.
The frontend is publicly accessible, the Spring Boot backend is connected through the Nginx reverse proxy, and MariaDB is configured as the application database.
Deployment URL: http://<EC2-PUBLIC-IP>
Deployment Status: Successful
7. Key Learnings
	• Deploying a full-stack application on Amazon EC2.
	• Building and running a Spring Boot JAR in a Linux environment.
	• Configuring MariaDB and application database connectivity.
	• Hosting React production builds using Nginx.
	• Configuring Nginx as a reverse proxy.
	• Managing backend applications using systemd.
	• Configuring AWS IAM roles and security groups.
	• Testing frontend and backend connectivity in a cloud environment.
