This repository deploys a sample architecture containing;
* Azure Kubernetes Services
* Azure API Management
* Azure Redis Cache Services
* Azure Container Registry
* SQL Azure
* CosmosDB
* and a few others....

The environment has been created leveraging virtual networks and private network connections as much as possible.  Take a look...

[![Visualize](/images/visualizebutton.svg?sanitize=true)](http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fbwatts64%2Ftemplates%2Fmaster%2Fazuredeploy.json)

You can start your deployment by clicking the button below.  You will be redirected to the Azure portal, asked to login, and asked to fill out a few parameters for the deployment.

[![Visualize](/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fbwatts64%2Ftemplates%2Fmaster%2Fazuredeploy.json)

You can also clone the repository locally and deploy the environment using the azure command line via;

    az deployment group create --resource-group <yourgroup> --template-file azuredeploy.json --paramters @azuredeploy.parameters.json


When you are done playing with the environmnet, you can simply delete the resource group to clean up.

