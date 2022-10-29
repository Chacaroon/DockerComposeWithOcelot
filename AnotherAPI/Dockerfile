FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 5002

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["AnotherAPI/AnotherAPI.csproj", "AnotherAPI/"]
RUN dotnet restore "AnotherAPI/AnotherAPI.csproj"
COPY . .
WORKDIR "/src/AnotherAPI"
RUN dotnet build "AnotherAPI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AnotherAPI.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AnotherAPI.dll"]
