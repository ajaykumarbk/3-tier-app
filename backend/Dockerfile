FROM node:alpine

WORKDIR /app
COPY app/package.json /app/

COPY app /app

RUN npm install	

EXPOSE 3000

CMD ["node","server.js"]
