### Note again this will run as root

````
# build environment
FROM node:13.12.0-alpine as build
WORKDIR /app
# Do we need the below? are are doing a production build and this is a build container
ENV PATH /app/node_modules/.bin:$PATH

COPY package.json ./
COPY package-lock.json ./
# do a clean install
RUN npm ci --silent
# We don't need the below because we are running a production build
# RUN npm install react-scripts@3.4.3 -g --silent
# Below can stay but I'm removing for now you should probably do this during development so you don't introduce a bug
# RUN npm audit fix
COPY . ./
RUN npm run build

# production environment
FROM nginx:stable-alpine
LABEL maintainer="Fabian Brash"
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
````
