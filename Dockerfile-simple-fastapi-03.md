## So I ran into a few issues with this so here is my fix

#### If you created a .dockerignore remove the line that omits .env files as we need that to connect to our DB

#### I really like that this runs as non-root this file is the base file that comes from vscode, with some mods ofcourse

#### Also I have tested this and it does work, not just an untested brain dump

##

[fastAPI docker docs](https://fastapi.tiangolo.com/deployment/docker/)


````
# For more information, please refer to https://aka.ms/vscode-docker-python
FROM python:3.8-slim-buster

EXPOSE 8000

# Keeps Python from generating .pyc files in the container
ENV PYTHONDONTWRITEBYTECODE=1

# Turns off buffering for easier container logging
ENV PYTHONUNBUFFERED=1

# Install pip requirements
COPY requirements.txt .
RUN python -m pip install --no-cache-dir --upgrade -r requirements.txt

WORKDIR /app
COPY ./app.py /app
COPY ./.env /app

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-python-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "py-fastapi-mongo" appuser && chown -R appuser /app
USER appuser

# During debugging, this entry point will be overridden. For more information, please refer to https://aka.ms/vscode-docker-python-debug
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]

# Docs say you can do this if TLS is being terminated by a proxy, i.e. nginx, traefik

#CMD ["uvicorn", "app:app", "--proxy-headers", "--host", "0.0.0.0", "--port", "80"]


````
