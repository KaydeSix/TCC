# Etapa base
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Etapa de build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copia toda a solução para garantir que os projetos referenciados sejam encontrados
COPY . . 

# Define a pasta correta onde está o .sln antes de restaurar
WORKDIR "/src"
RUN dotnet restore "TCC.sln"

# Compila o projeto EPonto
WORKDIR "/src/EPonto"
RUN dotnet build "./EPonto.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Etapa de publicação
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./EPonto.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Etapa final
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "EPonto.dll"]
