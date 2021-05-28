### Note check out the below base image

[fastapidocs](https://fastapi.tiangolo.com/deployment/docker/)

[fastapi-base-image](https://hub.docker.com/r/tiangolo/uvicorn-gunicorn-fastapi)

````
FROM tiangolo/uvicorn-gunicorn-fastapi:python3.7

COPY ./app /app
````
