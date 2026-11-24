# TP4 - Docker et Docker Compose (application MERN)

## Etapes realisees

1. Clonage du projet : dossiers Client (React/Vite) et Server (Express).
2. Dockerfile du serveur (node:lts-alpine, port 9000, npm start) puis :

       docker build -t mern-server ./Server

3. Dockerfile du client (node:lts-alpine, npm run build, serve sur le port 3000) puis :

       docker build -t mern-client ./Client

4. Creation du reseau et lancement de MongoDB :

       docker network create mern-network
       docker run -d --name mongodb --network mern-network mongo

5. Lancement manuel du serveur et du client :

       docker run -d --name server --network mern-network -p 9000:9000 mern-server
       docker run -d --name client --network mern-network -p 3000:3000 mern-client

6. docker-compose.yml avec 3 services (mongodb, server, client), le reseau
   mern-network (driver bridge) et la variable MONGO_URI. Lancement :

       docker compose up --build

7. Jenkinsfile : Checkout, Build, Run, Test, puis arret des services.

## Tests

- Serveur : http://localhost:9000/api/health renvoie {"message":"API is running"}
- Client : http://localhost:3000 affiche l'application React
- Jenkins automatise ces deux tests avec curl.
