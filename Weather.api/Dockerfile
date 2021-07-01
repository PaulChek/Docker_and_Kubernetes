FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS build
WORKDIR /src
COPY ["Weather.api/Weather.api.csproj","Weather.api/"]
RUN dotnet restore "Weather.api/Weather.api.csproj" #command uses NuGet to restore dependencies as well as project-specific tools that are specified in the project file.
COPY . . #first . is path outside the container and last . path is inside of container
WORKDIR "/src/Weather.api"
RUN dotnet build "Weather.api.csproj" -c Release -o /app/build #Builds a project and all of its dependencies.

FROM build AS publish 
RUN dotnet publish "Weather.api.csproj" -c Release -o /app/publish #Publishes the application and its dependencies to a folder for deployment to a hosting system 

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Weather.api.dll"] #tells Docker to configure the container to run as an executable.

#CMD ["bash", "-c", "apt update && apt install curl -y"]
RUN bash -c "apt update && apt install curl -y"