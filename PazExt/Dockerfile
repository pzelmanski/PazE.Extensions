FROM mcr.microsoft.com/dotnet/runtime:6.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

WORKDIR "/src"
COPY . .

# Params are in order to fix buildx step in github, failing on default dotnet restoreg
RUN dotnet restore -p:RestoreUseSkipNonexistentTargets=false -nowarn:msb3202,nu1503
RUN dotnet build "Paze.Extensions/Paze.Extensions.csproj" -c Release --no-restore -o /app/build

FROM build AS publish

RUN dotnet publish "Paze.Extensions/Paze.Extensions.csproj" -c Release -o /app/publish
RUN dotnet pack "Paze.Extensions/Paze.Extensions.csproj" --output nuget-packages --configuration Release

ARG nuget_api_key
RUN dotnet nuget push "nuget-packages/*.nupkg" --api-key ${nuget_api_key} --source "https://api.nuget.org/v3/index.json"
