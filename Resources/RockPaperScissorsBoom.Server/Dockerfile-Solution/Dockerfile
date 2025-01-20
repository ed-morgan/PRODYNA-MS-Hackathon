######################
# Build the build environment
######################

# We use the dotnet SDK image from microsoft as a base. This means we have all the tools needed to build the app
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

# Set working directory
WORKDIR /src

# Copy the csproj file from our app server from your local file system into the file system of the image
COPY ["RockPaperScissorsBoom.Server/RockPaperScissorsBoom.Server.csproj", "RockPaperScissorsBoom.Server/"]

# Dotnet restore
RUN dotnet restore "RockPaperScissorsBoom.Server/RockPaperScissorsBoom.Server.csproj"

# Copy all other files from your local to the file system
COPY . .

# Set current working directory to be be where the csproj file lives
WORKDIR "/src/RockPaperScissorsBoom.Server"

# Publish the app
RUN dotnet publish "RockPaperScissorsBoom.Server.csproj" -c Release -o /app/publish /p:UseAppHost=false

######################
# Build runtime image
######################

# This time we use the dotnet run time image from microsoft. This keeps the final image as lightwight as possible.
FROM mcr.microsoft.com/dotnet/aspnet:6.0

# Set working directory
WORKDIR /app

# Copy the files we published in the build environment step above to our new image.
# --from= command will allow you to reference other images within the dockerfile. `build` comes from the `AS build` at the top of the file. 
# Its possible to use any name you wish in place of `build` if you choose.
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "RockPaperScissorsBoom.Server.dll"]