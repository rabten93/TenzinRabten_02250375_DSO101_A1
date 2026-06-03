# TenzinRabten_02250375_DSO101_A1
# To-Do List  - Continuous Integration and Continuous Deployment (DSO101)
# Assignment 1

-------------------
Aim and Objectives
-------------------

To understand and implement CI/CD  using containerization and cloud platforms.
To build a full‑stack application.
Containerise the services with Docker and manage environment variables.
Push Docker images to a registry (Docker Hub) and manually deploy them on a cloud platform (Render.com) 
Automate the entire deployment using a render.yaml

---------------------------------------------------------------------------------------------------
Repository URL = https://github.com/rabten93/TenzinRabten_02250375_DSO101_A1.git
---------------------------------------------------------------------------------------------------

---------------------
Application Overview
---------------------

- Frontend : React.js
- Backend : Node.js + Express
- Database : PostgreSQL
- Deployment: Render.com 

# Live URL
https://fe-todo-dxu2.onrender.com


### Prerequisites
- Node.js (v18+)
- PostgreSQL installed locally 
- Docker
- Git

--------------------------
Implementation of steps
--------------------------
[Part 0]
- Set up project folders: backend/, frontend/.
- Install necessary dependencies
- Create PostgreSQL database locally (via pgAdmin4 or CREATE DATABASE tododb;).
- In backend Write server.js with CRUD endpoints and database connection.
- Create .env with local DB credentials (DB_HOST, DB_USER, etc.) and PORT=5000.
- Build frontend using react 
- App.js using axios to call backend API.
- Create .env with REACT_APP_API_URL=http://localhost:5000.
- Run locally
- Test all CRUD operations (add, edit, complete, delete tasks). Tasks persist in PostgreSQL.

[Part A]
Write Dockerfiles:
- Backend: FROM node:18-alpine, COPY package*.json ./, RUN npm install, COPY . ., EXPOSE 5000, CMD ["node","server.js"]
- Frontend: multi‑stage build (build stage with npm run build, then copy to nginx:alpine).
- Build Docker images:
- docker build -t chimirinzin/be-todo:02250346 ./backend
- docker build -t chimirinzin/fe-todo:02250346 ./frontend
- Push images to Docker Hub:
- docker push chimirinzin/be-todo:02250346
- docker push chimirinzin/fe-todo:02250346
On Render:
- Create a free PostgreSQL database (todo-db). 
-Create a Web Service for backend, frontend and database

[Part B]
- Create render.yaml defining backend and frontend services + database 
- Push everything to GitHub.
- Deploy using Render 



--------------------
How to run Locally?
--------------------
# Clone the repository
-In terminal
git clone https://github.com/rabten93/TenzinRabten_02250375_DSO101_A1.git
# Create database
psql -U postgres -c "CREATE DATABASE tododb;"

# Backend
cd backend
npm install
npm run dev         

# Frontend 
cd ../frontend
npm install
npm start         

-----------
Reflection
-----------

During this project, I created a CI/CD pipeline for a full-stack To-Do List application, advancing from local development to completely automated cloud deployment. In Part 0, I discovered the significance of environment variables and local testing, integrating React, Node.js, and PostgreSQL with effective CRUD operations.

In Part A, I utilized Docker to containerize both services, uploaded the images to Docker Hub using my student ID as the tag, and manually deployed everything to Render.com, which illustrated how cloud platforms handle databases and web services. 

In Part B, I streamlined the whole process by developing a render.yaml blueprint and linking Render directly to GitHub, enabling an automatic rebuild and redeployment of both frontend and backend with every git push. This task provided me with hands-on experience in current DevOps methodologies — containerization, CI/CD, infrastructure as code, and cloud deployment — and demonstrated the process of how applications move from a local development setting to a publicly accessible web application.

-----------------------
Problems and Solutions
-----------------------
Problem: Render spins down services after 15 minutes inactivity; first request takes ~50 seconds.
Solution: use uptime monitor (UptimeRobot) to keep alive.
 
Problem 2: Frontend not connecting to backend
Cause: set environment variable in frontend for REACT_APP_API_URL = Backend's URL


------------------
Learning Outcomes
------------------
- Set up a full‑stack application (frontend,backend,database)
- Configure environment variables (.env files) to store sensitive data
- Usage of (.gitignore).
- Connect frontend to backend using environment variables.
- Perform CRUD operations (Create, Read, Update, Delete) with a database.
- To Write Dockerfiles for both frontend and backend services.
- Build Docker images.
- Push images to a Docker Hub.
- Deploy a managed PostgreSQL database on Render.com.
- To Write a render.yaml blueprint to define infrastructure as code (multi‑service setup).
- Connect Render directly to your GitHub repository


