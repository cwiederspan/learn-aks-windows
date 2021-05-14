# Windows Container Time Zone Testing

## Tracking

* https://github.com/microsoft/Windows-Containers/issues/15
* https://github.com/Azure/AKS/issues/2295

## Build the Image

```bash
az acr build -r cdwms --platform windows --image wintestcoretz:10.0.17763.1935 -f Dockerfile-10.0.17763.1935 .
az acr build -r cdwms --platform windows --image wintestcoretz:10.0.18363.1556 -f Dockerfile-10.0.18363.1556 .
az acr build -r cdwms --platform windows --image wintestcoretz:10.0.19041.985  -f Dockerfile-10.0.19041.985  .

az acr build -r cdwms --platform windows --image wintestcoretz:10.0.17763.1935.ps -f Dockerfile-10.0.17763.1935.ps .

```