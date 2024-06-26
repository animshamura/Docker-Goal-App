### Project Architecture: 
<img src="https://github.com/animshamura/Three-Tier-Dockerization/blob/main/app-screenshots/archi.jpg?raw=true">

### Dockerization sections:  
   - Dockerfile 
   - Docker-compose file 
   - Micro service connection  
   - Output analysis
   
### Dockerfile: 
Dockerfile for the backend api. 
```
FROM node:16-alpine

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 5000

CMD [ "npm","start" ]
```
Dockerfile for the frontend application. 
```
FROM node:16-alpine as BUILDER

WORKDIR /app

COPY . .

RUN npm install

RUN npm run build


FROM nginx

COPY --from=BUILDER /app/build/ /usr/share/nginx/html

COPY ./conf/nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```
### Docker-compose file :
```
version: '3.8'
services:
  api:
    build: ./backend
    volumes: 
      - ./logs/:/app/logs/
    depends_on:
      - mongo

  client:
    build: ./frontend
    stdin_open: true
    tty: true
    depends_on: 
      - api

  nginx:
    build: ./nginx
    ports:
      - 80:80
    restart: always
    depends_on:
      - mongo
      - api
      - client
  mongo:
    image: mongo:latest
    container_name: mongo_db
    environment:
      - MONGO_INITDB_DATABASE=testdb
      - MONGO_INITDB_ROOT_USERNAME=root
      - MONGO_INITDB_ROOT_PASSWORD=root
  
    restart: unless-stopped
    ports:
      - "27017:27017"
    volumes:
      - ./my-data/db:/data/db
      - ./database/archive:/database/archive

  mongo-express:
    image: mongo-express
    container_name: mexpress
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=shamura
      - ME_CONFIG_MONGODB_ADMINPASSWORD=shamura
      - ME_CONFIG_MONGODB_URL=mongodb://root:root@mongo:27017/?authSource=admin
      - ME_CONFIG_BASICAUTH_USERNAME=anim
      - ME_CONFIG_BASICAUTH_PASSWORD=anim
   
    restart: unless-stopped
    ports:
      - "8081:8081"

volumes: 
  logs:
```
### Micro service connection: 

### Output analysis : 
Added 4 goals Spring Boot, Machine Learning, Deep Learning and DevOps respectively. <br/>  <br/> 
<img src="https://github.com/animshamura/Three-Tier-Dockerization/blob/main/app-screenshots/add-goals.png?raw=true"><br/> <br/> 
Removed Machine Learning from the goals' list. <br/>  <br/> 
<img src="https://github.com/animshamura/Three-Tier-Dockerization/blob/main/app-screenshots/remove-goals.png?raw=true"> <br/>  <br/> 
List of goals in the database through Mongo Express. <br/>  <br/> 
<img src="https://github.com/animshamura/Three-Tier-Dockerization/blob/main/app-screenshots/dbstate.png?raw=true">
 <br/> 
