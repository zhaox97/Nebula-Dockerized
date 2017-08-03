FROM node:7-alpine

RUN mkdir /www
WORKDIR /www

COPY ./Omniview/package.json /www
RUN npm install

COPY ./Omniview /www
EXPOSE 8080

CMD ["npm", "start"]

