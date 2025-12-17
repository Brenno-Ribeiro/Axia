# See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.


FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base

WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src


COPY ["Axia.slnx", "."]

COPY ["src/Axia.WebApi/Axia.WebApi.csproj", "src/Axia.WebApi/"]
COPY ["src/Axia.Application/Axia.Application.csproj", "src/Axia.Application/"]
COPY ["src/Axia.Domain/Axia.Domain.csproj", "src/Axia.Domain/"]
COPY ["src/Axia.Infrastructure/Axia.Infrastructure.csproj", "src/Axia.Infrastructure/"]


RUN dotnet restore "src/Axia.WebApi/Axia.WebApi.csproj"

COPY . .

WORKDIR /src
RUN dotnet build "src/Axia.WebApi/Axia.WebApi.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

RUN dotnet publish "src/Axia.WebApi/Axia.WebApi.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Axia.WebApi.dll"]
