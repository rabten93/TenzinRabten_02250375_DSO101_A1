# TenzinRabten_02250375_DSO101_A1
# README for DSO101_A1
# 02250375
## Aim
To containerise a full-stack To-Do application using Docker and deploy it on Render.com with automatic deployment using Render Blueprint.

## Objectives
- Create a Dockerfile for both frontend (React) and backend (Node.js/Express).
- Push both Docker images to DockerHub with student ID as tag.
- Set up a PostgreSQL database on Render.
- Configure a render.yaml Blueprint file for multi-service deployment.
- Deploy both services on Render.com such that they work together.

# BACKGROUND INFORMATION

Containerisation packages an application with all its dependencies so it runs consistently anywhere. This project uses Docker to containerise a full-stack To-Do app and Render Blueprint to deploy both frontend and backend services automatically.

## Why Docker?
Docker ensures the application runs the same way on my laptop, on a test server, and in production. For this assignment, I used Node.js Alpine base images to keep the containers small.

## Why Render Blueprint?
Render Blueprint uses a render.yaml file to define multiple services that deploy together. Every time I push code to GitHub, Render automatically redeploys both services.

## Security
Database credentials are stored in the render.yaml file as environment variables, not hard-coded in the application.

## PROCEDURES
Task 1: Backend Dockerfile

Created a Dockerfile in the backend folder:

FROM node:22-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY prisma ./prisma
RUN npx prisma generate

COPY . .

EXPOSE 5000

CMD ["sh", "-c", "npx prisma migrate deploy && node server.js"]

----

Task 2: Frontend Dockerfile

Created a Dockerfile in the frontend folder:

### Build stage
FROM node:22-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

### Production stage
FROM nginx:stable-alpine

COPY --from=builder /app/build /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

----

Task 3: Build and Push Docker Images

Built both images for the linux/amd64 platform (required by Render) and pushed to DockerHub.

### Build backend
docker build --platform linux/amd64 -t rabten93/be-todo:02250375 ./backend

### Push backend
docker push rabten93/be-todo:02250375

### Build frontend
docker build --platform linux/amd64 -t rabten93/fe-todo:02250375 ./frontend

### Push frontend
+docker push rabten93/fe-todo:02250375

----

<br>
Task 4: PostgreSQL Database on Render
Created a PostgreSQL database on Render with the following details:
<br>
Field	Value
<br>
Hostname	dpg-d7tm9mho3t8c739les90-a
<br>
Username	todo_db_dy2w_user
<br>
Password	R6R0w9EHZmfCQAXpHpC15Q3Npwehu5Nw
<br>
Database	todo_db_dy2w



----

Task 5: Render Blueprint File (render.yaml)

Created render.yaml in the repository root:

services:
  - type: web
    name: be-todo-api
    env: docker
    plan: free
    dockerfilePath: ./backend/Dockerfile
    dockerContext: ./backend
    envVars:
      - key: DATABASE_URL
        value: sha256:d97ae86adedf87b8704fc30a1a8bbf95b809e5e32b44162c3b74ae4ebc5d2689
      - key: PORT
        value: 5000

  - type: web
    name: fe-todo
    env: docker
    plan: free
    dockerfilePath: ./frontend/Dockerfile
    dockerContext: ./frontend
    envVars:
      - key: REACT_APP_API_URL
        value: https://be-todo-api.onrender.com


----

Task 6: Deploy on Render.com

Render → New + → Blueprint
Connected GitHub repository
Applied the configuration from render.yaml

Render automatically deployed both services.




## CONCLUSION

In this project, I successfully containerised a full-stack To-Do application using Docker. Separate Dockerfiles were created for the React frontend and Node.js backend. Both images were built and pushed to DockerHub using my student ID as the tag, and deployed on Render using a render.yaml Blueprint configuration.

One major challenge was building Docker images for the correct platform. Since my system uses ARM64 architecture, I had to explicitly specify platform linux/amd64 to ensure compatibility with Render’s infrastructure.

Another issue was enabling communication between the frontend and backend services. This was resolved by setting the REACT_APP_API_URL environment variable in the frontend service to point to the backend API.

Additionally, configuring environment variables and database connectivity required careful attention, especially formatting the PostgreSQL connection string correctly.

With everything configured, the deployment process is now fully automated. Any changes pushed to GitHub trigger a redeployment on Render, ensuring the application stays up to date. The final system is live, functional, and follows a proper CI/CD workflow.
