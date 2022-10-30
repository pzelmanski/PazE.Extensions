FROM mcr.microsoft.com/dotnet/runtime:6.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["PazExt/PazExt.csproj", "PazExt/"]
RUN dotnet restore "PazExt/PazExt.csproj"
COPY . .
WORKDIR "/src/PazExt"
RUN dotnet build "PazExt.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "PazExt.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PazExt.dll"]
