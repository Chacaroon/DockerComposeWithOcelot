FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
ARG APP_PORT
EXPOSE ${APP_PORT}

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Gateway/Gateway.csproj", "Gateway/"]
RUN dotnet restore "Gateway/Gateway.csproj"
COPY . .
WORKDIR "/src/Gateway"
ARG APP_CONFIGURATION
RUN dotnet build "Gateway.csproj" -c ${APP_CONFIGURATION} -o /app/build

FROM build AS publish
RUN dotnet publish "Gateway.csproj" -c ${APP_CONFIGURATION} -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS "http://+:${APP_PORT}"
ENTRYPOINT ["dotnet", "Gateway.dll"]
