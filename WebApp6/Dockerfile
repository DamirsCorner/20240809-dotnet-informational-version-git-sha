#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
ARG GIT_SHA
WORKDIR /src
COPY ["WebApp6/WebApp6.csproj", "WebApp6/"]
RUN dotnet restore "./WebApp6/WebApp6.csproj"
COPY . .
WORKDIR "/src/WebApp6"
RUN dotnet build "./WebApp6.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./WebApp6.csproj" -c $BUILD_CONFIGURATION -p:SourceRevisionId=$GIT_SHA -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApp6.dll"]