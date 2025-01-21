## Notes & Guidance

### Create the Logic App

1. Edit `workflow.json`: replace `[CHANGE-THIS-TO-YOUR-HOST]` to the host of you app container, e.g. `my-container-app.redtree-981eb8d2.westeurope.azurecontainerapps.io`

1.  Run the following Azure CLI command to create the Logic App (note that the `workflow.json` file path is inside the `/Resources` directory)

    ```shell
    az logic workflow create --name <logicapp-name> --resource-group <resource-group-name> --location germanywestcentral --definition workflow.json
    ```

    > Notes: 
    > - You can also use the Azure Portal to create the Logic App.
    > - The CLI may need to install the `logic` extension.  Choose `Y` to continue.



1.  Open the Logic App in the Azure Portal and click on the `Development Tools` option on the side bar, and then on `Logic App Designer` option in the submenu.

1.  Click on the `Add a trigger` option in the middle of the screen. Search for `recurrence` and select it as trigger type.

1.  Set the recurrence to run every 5 minutes. Add an action to the trigger by clicking the small `+` symbol.

1.  Search for `HTTP` action, and click on the `HTTP` action and set the `Method` to `POST`.

1.  Set the `URI` to the API URL of your Rock Paper Scissors Boom Server app with the API path appended to the end.

    ```shell
    https://<CONTAINER-APP-NAME>.<GENERATED-CODE>.westeurope.azurecontainerapps.io/api/rungame
    ```

1.  Click `Save`.

### Test the Logic App

1.  Click `Run Trigger` button to test

1.  Navigate to the web app, click on the `Run the Game` button and start clicking on the refresh button to see the game being played.
