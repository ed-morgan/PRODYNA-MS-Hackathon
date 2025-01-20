## Notes & Guidance

### Create Azure SQL Server & Database

1. Login to AZ CLI using the following command and the provided microsoft login credentials

    ```
    az login
    ```

1.  Create an Azure SQL Server via the Azure CLI.

    https://learn.microsoft.com/en-us/cli/azure/sql?view=azure-cli-latest

1.  Add an IP firewall rule to restrict access to just your local IP & Azure (PowerShell). This command should provide your IP Address.

    ```powershell
    $myIP = $(Invoke-WebRequest -Uri "https://api.ipify.org").Content
    ```

    https://learn.microsoft.com/en-us/cli/azure/sql/server/firewall-rule?view=azure-cli-latest

1.  Create an Azure SQL Database via the Azure CLI.

    https://learn.microsoft.com/en-us/cli/azure/sql/db?view=azure-cli-latest

1.  Run the following Azure CLI command to get the connection string for the Azure SQL database.

    https://learn.microsoft.com/en-us/cli/azure/sql/db?view=azure-cli-latest

### Modify the `docker-compose.yaml` file to point to the new Azure SQL database

1.  Open the `docker-compose.yaml` file in a text editor.

1.  Comment out the `rockpaperscissors-sql` service since the application will use an Azure SQL database.

1.  Modify the `rockpaperscissors-server` service to use the Azure SQL database (notice that you modify the connection string & remove the dependency on the `rockpaperscissors-sql` service). Get the server FQDN & database name from the Azure portal.

### Build & run the application

1.  Shut down the previous version of the application (to stop the local database).

    ```shell
    docker compose down
    ```

1.  Run the following command to build & run the application.

    ```shell
    docker compose up --build --remove-orphans
    ```

1.  Navigate to the locally running application in the browser (http://localhost) & play the game

### Check for records in the database

1.  Open the Azure portal (https://portal.azure.com).

1.  Navigate to the **Azure SQL Server** & **SQL Database**.

1.  Open the **Query Editor** & login with the credentials you specified earlier.

1.  Run the following query to see the records in the `GameRecords` table.

    ```sql
    SELECT * FROM [dbo].[GameRecords]
    ```

### Troubleshooting

- If the Docker image does not start correctly, it may be because it cannot access the database. Verify that connections can be made to the server. By default, the server will restrict access to the database. Add a firewall rule.
