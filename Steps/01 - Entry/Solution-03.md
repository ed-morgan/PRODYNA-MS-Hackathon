## Notes & Guidance

### Update application with DB connection string

1. We must update the `appsettings.json` to include the connection string for the database. We must update the following in the file:

```
  "ConnectionStrings": {
    "DefaultConnection": "<CONNECTION STRING GOES HERE>"
  },
````

### Create the Azure Container Registry & push your local Docker image to it

1.  Run the following Azure CLI to create the Azure Container Registry.

    ```shell
    az acr create -g <resource-group-name> -n <acr-name> --sku Basic --admin-enabled true
    ```

1.  Run the following Azure CLI to get the login server for the Azure Container Registry.

    ```shell
    az acr login -n <acr-name>
    ```

1.  Run the following Docker CLI to tag your local Docker image with the Azure Container Registry login server. **Note** that you may have slightly different image names. Check the list of images names created locally with `docker images`.

    ```shell
    docker tag rpsb-rockpaperscissors-server <acr-name>.azurecr.io/rockpaperscissors-server:latest
    ```

1.  Run the following Docker CLI to push your local Docker image to the Azure Container Registry.

    ```shell
    docker push <acr-name>.azurecr.io/rockpaperscissors-server:latest
    ```

### Create the Azure Container Apps & deploy


1.  Create and deploy your first container app with the containerapp up command. This command will:

    Create the Container Apps environment
    Create the Log Analytics workspace
    Create and deploy the container app using a public container image
    Note that if any of these resources already exist, the command will use them instead of creating new ones.

    ```shell
    az containerapp up --name my-container-app --resource-group my-container-apps --location westeurope --environment 'my-container-apps' --image <Our image from the ACR above> --target-port 80 --ingress external --query properties.configuration.ingress.fqdn
    ```

    By setting --ingress to external, you make the container app available to public requests.


### Test your web app

1.  Navigate to your web app in a browser and play a game. You can get the url from the Azure portal (e.g., https://<CONTAINER-APP-NAME>.<GENERATED-CODE>.westeurope.azurecontainerapps.io).

1.  Login to the database (using the Query editor in the Azure portal) to ensure you are getting new database records

    ```sql
    SELECT * FROM [dbo].[GameRecords]
    ```

### Troubleshooting

- If the application does not start correctly, make sure the database firewall is not restricting access. To check this, in the Azure Portal, navigate to the **Azure SQL** -> **Networking** page and check the box **"Allow Azure services and resources to access this server."**
