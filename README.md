### Section:  
   - Dockerfile 
   - Docker-compose file 
   - Micro service connection  
   - Output analysis
### Dockerfile: 
```
FROM node:16-alpine

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 5000

CMD [ "npm","start" ]
```
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
### Output analysis :

     
