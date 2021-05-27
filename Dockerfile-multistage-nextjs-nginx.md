### Note the below will run the container as root

````
# build environment
FROM node:13.12.0-alpine as build
LABEL maintainer="Fabian Brash"
WORKDIR /app
ENV PATH /app/node_modules/.bin:$PATH
COPY package.json ./
COPY package-lock.json ./
# do a clean install
RUN npm ci --silent
# this fixes a bug with newer versions of react-scripts & docker
# RUN npm install react-scripts@3.4.3 -g --silent
# RUN npm audit fix
COPY . ./
RUN npm run build

# production environment
FROM nginx:stable-alpine
COPY --from=build /app/out /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
````
