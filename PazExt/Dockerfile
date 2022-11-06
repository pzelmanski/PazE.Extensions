FROM mcr.microsoft.com/dotnet/runtime:6.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

WORKDIR "/src"
COPY . .

RUN dotnet restore "Paze.Extensions/Paze.Extensions.csproj"
RUN dotnet build "Paze.Extensions/Paze.Extensions.csproj" -c Release --no-restore -o /app/build

FROM build AS publish

RUN dotnet publish "Paze.Extensions/Paze.Extensions.csproj" -c Release -o /app/publish
RUN dotnet pack "Paze.Extensions/Paze.Extensions.csproj" --output nuget-packages --configuration Release

ARG nuget_api_key
RUN dotnet nuget push "nuget-packages/*.nupkg" --api-key ${nuget_api_key} --source "https://api.nuget.org/v3/index.json"
