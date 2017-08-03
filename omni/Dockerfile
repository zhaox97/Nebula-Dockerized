FROM node:7-alpine

RUN mkdir /www
WORKDIR /www

COPY ./src/package.json /www
RUN npm install

COPY ./src /www
EXPOSE 8080

CMD ["npm", "start"]

