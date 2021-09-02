## Note we have already done our production build and we are just copying that to inetpub to serve it up.

````
ARG iiscore=mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2019
FROM $iiscore

RUN powershell -NoProfile -Command Remove-Item -Recurse C:\inetpub\wwwroot\*

WORKDIR /inetpub/wwwroot

COPY dist/ .
````
