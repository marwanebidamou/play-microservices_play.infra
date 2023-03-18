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

## Creating the Cosmos DB Account
```powershell
$dbaccountname="playeconomydbazure" #must be unique globally
az cosmosdb create --name $dbaccountname --resource-group $appname --kind MongoDB --enable-free-tier
```

## Creating the Service Bus namespace
```powershell
$azservicebusnamespace="azservicebuseconomy"
az servicebus namespace create --name $azservicebusnamespace --resource-group $appname --sku Standard
```

## Creating the Container Registry
```powershell
$acrname="azacreconomy"
az acr create --name $acrname --resource-group $appname --sku Basic
```

## Creating the Azure Kubernetes Service (AKS) cluster
```powershell
az extension add --name aks-preview
az feature register --name "EnableWorkloadIdentityPreview" --namespace "Microsoft.ContainerService"

# Wait for this command to reach a "Registered" state (it may take up to 15min)
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/EnableWorkloadIdentityPreview')].{Name:name,State:properties.state}"

az provider register --namespace Microsoft.ContainerService

az aks create -n $appname -g $appname --node-vm-size Standard_B2s --node-count 2 --attach-acr $acrname --enable-oidc-issuer --enable-workload-identity --generate-ssh-keys

az aks get-credentials --name $appname --resource-group $appname
```