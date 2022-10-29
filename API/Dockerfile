FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
ARG APP_PORT
EXPOSE ${APP_PORT}

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["API/API.csproj", "API/"]
RUN dotnet restore "API/API.csproj"
COPY . .
WORKDIR "/src/API"
ARG APP_CONFIGURATION
RUN dotnet build "API.csproj" -c ${APP_CONFIGURATION} -o /app/build

FROM build AS publish
RUN dotnet publish "API.csproj" -c ${APP_CONFIGURATION} -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS "http://+:${APP_PORT}"
ENTRYPOINT ["dotnet", "API.dll"]
