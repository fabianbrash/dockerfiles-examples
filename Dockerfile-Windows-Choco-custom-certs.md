## Let's build a custom Windows image and install Chocolatey, note we are installing certs for deep packet inspection, most Corps do this so you must add them to the base image

````
FROM mcr.microsoft.com/windows/servercore:ltsc2019

WORKDIR /

COPY mycert01.cer .

COPY mycert02.cer .

RUN powershell -NoProfile -Command Import-Certificate -FilePath ./mycert01.cer -CertStoreLocation Cert:\LocalMachine\Root

RUN powershell -NoProfile -Command Import-Certificate -FilePath ./mycert02.cer -CertStoreLocation Cert:\LocalMachine\Root

RUN powershell -NoProfile -Command Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
````
