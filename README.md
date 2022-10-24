[![Run Azure Login with OpenID Connect](https://github.com/azure-expert/azure-login-gh-actions/actions/workflows/main.yml/badge.svg)](https://github.com/azure-expert/azure-login-gh-actions/actions/workflows/main.yml)

## Use the Azure login action with a service principal secret
To use Azure login with a service principal, you first need to add your Azure service principal as a secret to your GitHub repository.

### In this example, you will create a secret named AZURE_CREDENTIALS that you can use to authenticate with Azure.

1- Open Azure Cloud Shell in the Azure portal or Azure CLI locally.
`$ az login`

2- Before to proceed with others commands, please make sure you have the following:

` az group create -n maria -l brazilsouth`

3- Create a new service principal in the Azure portal for your app. The service principal must be assigned the Contributor role.

```bash
az ad sp create-for-rbac --name "myApp" --role contributor \
                                --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} \
                                --sdk-auth
```


4- Copy all the JSON object for your service principal.

```bash
{
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
}
```

5- Open your GitHub repository and go to Settings and add on the Secrets session the `AZURE_CREDENTIALS`.

6- Paste in your JSON object for your service principal with the name `AZURE_CREDENTIALS`

## The Action to be used

```yml
on: [push]

name: AzureLoginSample

jobs:
build-and-deploy:
    runs-on: ubuntu-latest
    steps:

    - name: Log in with Azure
        uses: azure/login@v1
        with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Azure CLI script
        uses: azure/CLI@v1
        with:
        azcliversion: 2.0.72
        inlineScript: |
            az account show
            az storage -h
```

## References
[Authenticate  from Azure to Actions](https://learn.microsoft.com/en-us/azure/developer/github/connect-from-azure?tabs=azure-portal%2Clinux)

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)


