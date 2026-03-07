FROM node:20-alpine

ENV MONGO_DB_USERNAME=ADMIN \
    MONGO_DB_PWD=password

RUN mkdir -p /home/app

COPY . /home/app

CMD ["node", "server.js"]