# client/Dockerfile
FROM node:14 as build

WORKDIR /app

COPY package.json ./
COPY package-lock.json ./
RUN npm install

COPY . .

RUN npm run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d

ENV REACT_APP_API_URL=http://localhost:8000

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]