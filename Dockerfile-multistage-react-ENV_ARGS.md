````
FROM node:14.15.1-alpine3.12 AS build
WORKDIR /usr/src/app
COPY ["package.json", "package-lock.json*", "npm-shrinkwrap.json*", "./"]
# Install our dependencies
RUN npm ci --production --silent
# Copy our code note you need a .dockerignore file so you are not copying things like node_modules etc.
COPY . .

# Before building our app we need to add our ENV variable so we can have our API key
ARG API_KEY=default_value
ENV REACT_APP_NASA_KEY=${API_KEY}

# Build our app
RUN npm run build

FROM node:14.15.1-alpine3.12
LABEL maintainer="Fabian Brash"
COPY --from=build /usr/src/app/build/ /opt/app
RUN npm install -g serve --silent
WORKDIR /opt/app
USER 1000
EXPOSE 5000
#Be explicit serve 13.x now defaults to 3000 instead of 5000
CMD ["serve", "-p", "5000"]
````

### And now our build command will become

````
docker build --build-arg API_KEY=r05fhg... -t local/myimage:0.0.1 .
````
