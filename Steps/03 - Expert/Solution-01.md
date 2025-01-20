## Notes & Guidance

1.  Navigate to the `Resources` directory.

1.  Navigate to  `RockPaperScissorsBoom.Server` directory within `Resources`

1. Create `Dockerfile`

1.  Here is the outline of the Dockerfile contents using comments. Have a go at building the Dockerfile out! If you need the solution, you can find it under `Dockerfile-solution` directory. There is two versions of this file, with `Dockerfile` being the most straight forward, and the `Dockerfile-optimal` being a more complex multi-stage version.

1.  Here we will paste the following code. The comments should explain what each line does.

```
######################
# Build the build environment
######################

# We use the dotnet SDK image from microsoft as a base. This means we have all the tools needed to build the app
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

# Set working directory to /src

# Copy the RockPaperScissorsBoom.Server.csproj file from our app server from your local file system into the RockPaperScissorsBoom.Server/ directory of the image

# Dotnet restore RockPaperScissorsBoom.Server/RockPaperScissorsBoom.Server.csproj

# Copy all other files from your local to the file system (Don't need to exclude csproj file from this, just copy everything)

# Set current working directory to be be where the RockPaperScissorsBoom.Server.csproj file lives

# Publish the app to /app/publish using /p:UseAppHost=false parameter

######################
# Build runtime image
######################

# This time we use the dotnet run time image from microsoft. This keeps the final image as lightwight as possible.
FROM mcr.microsoft.com/dotnet/aspnet:6.0

# Set working directory to /app

# Copy the files we published in the build environment step above to our new image.
# --from= command will allow you to reference other images within the dockerfile. `build` comes from the `AS build` at the top of the file. 
# Its possible to use any name you wish in place of `build` if you choose.

ENTRYPOINT ["dotnet", "RockPaperScissorsBoom.Server.dll"]
```

1.  In the `docker-compose.yaml` file in the `resource` directory, update the password for the SQL Server & the connection string for the database in the web app.

    - There are 3 places in the Docker Compose file where you need to update the password for the SQL Server
      - The environment variables for the database container
      - The health check for the database container
      - The environment variables for the web app container

1.  Run the following command to build & run the application & database.

    ```shell
    docker compose up --build -d
    ```

1.  Run the following command to check and see if the 2 Docker images (application & database) are running successfully.

    ```shell
    docker ps
    ```

    You should see something like this:

    ```shell
    CONTAINER ID   IMAGE                                        COMMAND                  CREATED          STATUS          PORTS                    NAMES
    396c8e105616   rpsb-rockpaperscissors-server                "dotnet RockPaperSci…"   19 minutes ago   Up 19 minutes   0.0.0.0:80->80/tcp       rockpaperscissors-server
    9f5ab21008de   mcr.microsoft.com/mssql/server:2022-latest   "/opt/mssql/bin/perm…"   40 minutes ago   Up 40 minutes   0.0.0.0:1433->1433/tcp   rockpaperscissors-sql
    ```

1.  Navigate to `http://localhost` in your browser.

1.  Play a game of Rock Paper Scissors Boom (click on the **Run the Game** link at the top of the page & click the **Run the Game** button).

    > NOTE: The following pages will result in an error since Azure AD B2C authentication has not been set up yet:
    >
    > 1. Sign In
    > 1. Competitors -> Create New
    > 1. Competitors -> <any-player-name> -> Edit | Delete

## Troubleshooting

- Make sure the students have the latest versions of Docker & Docker Compose installed on their machines.
  - Docker: `docker --version`
  - Docker Compose: `docker compose version`
