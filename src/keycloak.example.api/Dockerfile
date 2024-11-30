FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/keycloak.example.api/keycloak.example.api.csproj", "src/keycloak.example.api/"]
RUN dotnet restore "src/keycloak.example.api/keycloak.example.api.csproj"
COPY . .
WORKDIR "/src/src/keycloak.example.api"
RUN dotnet build "keycloak.example.api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "keycloak.example.api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "keycloak.example.api.dll"]
