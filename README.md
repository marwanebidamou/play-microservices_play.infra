# play.infra
Play Economy Infrastructure components

## Add the Github package source
```powershell
$owner="play-microservice"
$gh_pat="[GITHUB ACCESS TOKEN HERE]"
$nuget_src_name="Play Github"
dotnet nuget add source --username USERNAME --password $gh_pat --store-password-in-clear-text --name $nuget_src_name "https://nuget.pkg.github.com/$owner/index.json"
```
### List all nuget sources
```powershell
dotnet nuget list source
```

## Connecting to azure via azure cli
```powershell
#Login
az login
#Setting default subscription
az account set --subscription "Pay-as-you-go"
```

## Creating the Azure resource group
```powershell
$appname="playeconomy"
az group create --name $appname --location eastus
```