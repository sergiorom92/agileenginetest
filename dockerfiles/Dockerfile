FROM node:lts-alpine as build-stage

WORKDIR /app

COPY ../package*.json /app

RUN npm install

COPY ../ /app

RUN npm run build

# production stage
FROM nginx:stable-alpine as production-stage

COPY --from=build-stage /app/dist /usr/share/nginx/html

EXPOSE $PORT

CMD ["nginx", "-g", "daemon off;"]
