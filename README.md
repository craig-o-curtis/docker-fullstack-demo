# Quick and Dirty Fullstack Docker

Project Contains:
- Frontend in `client` dir
- Backend in `api` dir

## api Dockerfile
- limit package.json to /api/ folder
- make necessary changes to reference api folder
```dockerfile
FROM node

WORKDIR /app

COPY package*.json /app/

RUN npm install

COPY . /app/

EXPOSE 4000
CMD [ "npm", "start"]


```

## client Dockerfile
```dockerfile
FROM node

WORKDIR /client

COPY package*.json /client/

RUN npm install

COPY . /client/

EXPOSE 3000
CMD [ "npm", "start"]
```

## Docker Compose
- `docker-compose.yml` at root

```yml
version: "3"
services:
  app:
    container_name: app
    restart: always
    build: ./api
    ports:
      - "4000:4000"
    depends_on:
      - mongo
  client:
    container_name: client
    restart: always
    build: ./client
    ports:
      - "3000:3000"
    links:
      - app
  mongo:
    container_name: mongo
    image: mongo
    restart: always
    expose:
      - "27017"
    volumes:
      - ./data:/data/db
    ports:
      - "27017:27017"
```

## Building Images
- Check no images running with `docker ps`
- Stop running images with `docker stop <image-id>`
- Check previous images based on these with `docker images`
- Build docker-compose file with `docker-compose build`
- Start each service separately:
-- `docker-compose up -d mongo` 
-- `docker-compose up -d app` 
-- `docker-compose up -d client` 
- Check urls to make sure running
-- api at `localhost:4000`
-- front-end at `localhost:3000`

