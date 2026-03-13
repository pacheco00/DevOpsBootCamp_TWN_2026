FROM node:20-alpine

ENV MONGO_DB_USERNAME=ADMIN \
    MONGO_DB_PWD=password

RUN mkdir -p /home/app

COPY ./js-app/app /home/app

WORKDIR /home/app

RUN npm install

CMD ["node", "server.js"]