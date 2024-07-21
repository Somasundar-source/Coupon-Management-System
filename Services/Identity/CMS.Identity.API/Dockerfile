#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Services/Identity/CMS.Identity.API/CMS.Identity.API.csproj", "Services/Identity/CMS.Identity.API/"]
RUN dotnet restore "./Services/Identity/CMS.Identity.API/CMS.Identity.API.csproj"
COPY . .
WORKDIR "/src/Services/Identity/CMS.Identity.API"
RUN dotnet build "./CMS.Identity.API.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./CMS.Identity.API.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CMS.Identity.API.dll"]