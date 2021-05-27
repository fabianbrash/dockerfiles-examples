### Note this assumes all your .js,.css,.html are in the cwd again a .dockerignore is super important so you don't bring in anything you don't need

````
FROM node:13.12.0-alpine
LABEL maintainer="Fabian Brash"
WORKDIR /app
COPY . ./
RUN npm i -g serve

# let's run as non-root
USER 1000
EXPOSE  5000
CMD ["serve", "-s"]
````
