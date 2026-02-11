📘 Full Stack Book Store App (Dockerized)

This project is a Full Stack Book Store Application built using:

Frontend: React + Apache (Dockerized)

Backend: Node.js + Express (Dockerized)

Database: MySQL (optional / external)

Deployment: Docker on AWS EC2

🏗️ Architecture Overview
Browser
   |
   |----> Frontend (React + Apache) : Port 80
                    |
                    |----> Backend (Node.js API) : Port 81
                                   |
                                   |----> Database (MySQL)

📂 Project Structure
2nd10WeeksofCloudOps-main/
│
├── backend/        # Node.js backend
│   ├── Dockerfile
│   ├── index.js
│   ├── package.json
│   └── .env
│
├── client/         # React frontend
│   ├── Dockerfile
│   ├── public/
│   └── src/
│       └── pages/
│           └── config.js
│
├── docker-compose.yaml
└── README.md
🗄️ Database Setup (AWS RDS – MySQL)

This project uses AWS RDS (MySQL) as the database.

1️⃣ Create RDS MySQL Instance

Go to AWS Console → RDS

Create MySQL database

Choose:

Engine: MySQL

DB instance identifier: bookdb (any name)

Username & Password (note it down)

Make sure:

Public access = Yes

Security Group allows port 3306

Launch RDS and wait till status is Available

2️⃣ Connect RDS using MySQL Workbench

Open MySQL Workbench

Create new connection

Enter:

Hostname: RDS Endpoint

Port: 3306

Username: RDS username

Password: RDS password

Test connection → ✅ Connected

3️⃣ Create Database & Tables

After connecting through Workbench:

Open the file:

backend/test.sql


Copy entire SQL code

Paste it into MySQL Workbench query editor

Execute the query

This will:

Create database

Create books table

Insert sample data

4️⃣ Verify Database

Run:

USE books;
SELECT * FROM books;


If data is visible → Database setup successful ✅

 
⚙️ Prerequisites 
RDS DATABASES 

AWS EC2 (Amazon Linux)

Docker

Git

Ports open in Security Group:

80 → Frontend

81 → Backend

🚀 Installation & Setup
1️⃣ Install Docker & Git
sudo su -
yum install docker -y
systemctl start docker
yum install git -y

2️⃣ Clone Repository
git clone https://github.com/CloudTechDevOps/2nd10WeeksofCloudOps-main.git
cd 2nd10WeeksofCloudOps-main
5️⃣ Configure Backend to Use RDS

Create .env file inside backend/ directory:

DB_HOST=<RDS-ENDPOINT>
DB_USER=<RDS-USERNAME>
DB_PASSWORD=<RDS-PASSWORD>
DB_NAME=books

Example:
DB_HOST=bookdb.xxxxxx.us-east-1.rds.amazonaws.com
DB_USER=admin
DB_PASSWORD=********
DB_NAME=books

Backend will automatically connect to RDS using these values.

🔧 Backend Setup
3️⃣ Build Backend Image
cd backend
vi Dockerfile
docker build -t backend .

4️⃣ Run Backend Container
docker run -dt -p 81:3000 backend

5️⃣ Test Backend
http://<EC2-PUBLIC-IP>:81/books


You should see JSON data.

🎨 Frontend Setup
6️⃣ Configure API URL

Edit file:

client/src/pages/config.js

const API_BASE_URL = "http://<EC2-PUBLIC-IP>:81";
export default API_BASE_URL;

7️⃣ Build Frontend Image
cd client
vi Dockerfile
docker build -t frontend .

8️⃣ Run Frontend Container
docker run -dt -p 80:80 frontend

🌐 Access Application

Frontend UI

http://<EC2-PUBLIC-IP>


Backend API

http://<EC2-PUBLIC-IP>:81/books

🐳 Docker Containers Status

Check running containers:

docker ps


Expected output:

frontend  → 0.0.0.0:80->80
backend   → 0.0.0.0:81->3000

✅ Features

View all books

Add new book

Update book

Delete book

Dockerized frontend & backend

Ready for CI/CD & Kubernetes

🧠 Notes

Backend runs internally on port 3000

Apache serves React build on port 80

Frontend talks to backend using API_BASE_URL
