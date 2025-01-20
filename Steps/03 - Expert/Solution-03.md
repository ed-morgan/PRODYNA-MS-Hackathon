## Notes & Guidance

### Create the Azure Container Registry & push your local Docker image to it

1.  Run the following Azure CLI to create the Azure Container Registry.

    https://learn.microsoft.com/en-us/cli/azure/acr?view=azure-cli-latest

1.  Run the following Azure CLI to get the login server for the Azure Container Registry.

    https://learn.microsoft.com/en-us/cli/azure/acr?view=azure-cli-latest

1.  Run the following Docker CLI to tag your local Docker image with the Azure Container Registry login server. **Note** that you may have slightly different image names. Check the list of images names created locally with `docker images`.

    https://docs.docker.com/reference/cli/docker/image/

1.  Run the following Docker CLI to push your local Docker image to the Azure Container Registry.

    https://docs.docker.com/reference/cli/docker/image/

### Create the Azure Container Apps & deploy

1. We must update the `appsettings.json` to include the connection string for the database. We must update the following in the file:

    ```
    "ConnectionStrings": {
        "DefaultConnection": "<CONNECTION STRING GOES HERE>"
    },
    ````

1.  Create and deploy your first container app with the containerapp up command. This command will:

    Create the Container Apps environment
    Create the Log Analytics workspace
    Create and deploy the container app using a public container image
    Note that if any of these resources already exist, the command will use them instead of creating new ones.

    https://learn.microsoft.com/en-us/cli/azure/containerapp?view=azure-cli-latest#az-containerapp-up

    By setting --ingress to external, you make the container app available to public requests.


### Test your web app

1.  Navigate to your web app in a browser and play a game. You can get the url from the Azure portal (e.g., https://\<app-service-name\>.azurewebsites.net).

1.  Login to the database (using the Query editor in the Azure portal) to ensure you are getting new database records

    ```sql
    SELECT * FROM [dbo].[GameRecords]
    ```

### Troubleshooting

- If the application does not start correctly, make sure the database firewall is not restricting access. To check this, in the Azure Portal, navigate to the **Azure SQL** -> **Networking** page and check the box **"Allow Azure services and resources to access this server."**
