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
