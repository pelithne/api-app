# API APP Tutorial

## Pre-work:

Get dotnet.core. Instructions here for ubuntu 18.04: 
https://dotnet.microsoft.com/download/linux-package-manager/ubuntu18-04/sdk-2.1.201

Clone repo with basic dotnet core app
````
git clone https://github.com/Azure-Samples/dotnet-core-api
````

## Tutorial

### Run the application locally. 
````
cd dotnet-core-api
dotnet restore
dotnet run
````

This will start an endpoint on localhost. Navigate to `http://localhost:5000/swagger/` to see the swagger definition of the API.

To view one of the API endpoints, you can navigate to `http://localhost:5000/api/todo`

### Deploy to azure

#### Configure local git deployment
In the Cloud Shell, configure deployment credentials with the az webapp deployment user set command. This deployment user is required for FTP and local Git deployment to a web app. The user name and password are account level. They are different from your Azure subscription credentials.
In the following example, replace <username> and <password> (including brackets) with a new user name and password. The user name must be unique within Azure. The password must be at least eight characters long, with two of the following three elements: letters, numbers, symbols. 

````
az webapp deployment user set --user-name <username> --password <password>
````

#### Create a resource group
A resource group is a logical container into which Azure resources like web apps, databases, and storage accounts are deployed and managed. For example, you can choose to delete the entire resource group in one simple step later.
In the Cloud Shell, create a resource group with the az group create command. The following example creates a resource group named myResourceGroup in the West Europe location. To see all supported locations for App Service in Free tier, run the az appservice list-locations --sku FREE command.

````
az group create --name myResourceGroup --location "West Europe"
````

#### Create an App Service plan
In the Cloud Shell, create an App Service plan with the az appservice plan create command.
The following example creates an App Service plan named myAppServicePlan in the Free pricing tier:

````
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
````

#### Create a web app
Create a web app in the myAppServicePlan App Service plan. 
In the Cloud Shell, you can use the az webapp create command. In the following example, replace <app_name> with a globally unique app name (valid characters are a-z, 0-9, and -). 

````
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --deployment-local-git
````

#### Push to Azure from Git
Back in the local terminal window, add an Azure remote to your local Git repository. Replace <deploymentLocalGitUrl-from-create-step> with the URL of the Git remote that you saved from Create a web app.

````
git remote add azure <deploymentLocalGitUrl-from-create-step>
git push azure master
````

#### Browse to the Azure app
Navigate to `http://<app_name>.azurewebsites.net/swagger` in a browser and play with the Swagger UI.


## maybe not cors 
### Enable CORS
In the Cloud Shell, enable CORS to your client's URL by using the az resource update command. Replace the <appname> placeholder.

````
az resource update --name web --resource-group myResourceGroup --namespace Microsoft.Web --resource-type config --parent sites/<app_name> --set properties.cors.allowedOrigins="['http://localhost:5000']" --api-version 2015-06-01
````

TODO: Enable AD authentication
