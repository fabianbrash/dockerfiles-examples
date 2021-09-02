ARG iiscore=mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2019
ARG nodecore=local/srv2019-node:16.8.0
FROM $nodecore as build

LABEL maintainer="Fabian Brash"

WORKDIR /build

COPY package.json ./

COPY package-lock.json ./

RUN npm ci --silent

COPY . ./

RUN npm run build

FROM $iiscore 

LABEL maintainer="Fabian Brash"

RUN powershell -NoProfile -Command Remove-Item -Recurse C:\inetpub\wwwroot\*

WORKDIR /inetpub/wwwroot

COPY --from=build /build/dist/ .
