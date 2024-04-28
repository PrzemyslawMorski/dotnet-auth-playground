FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["WebApp_UnderTheHood/WebApp_UnderTheHood.csproj", "WebApp_UnderTheHood/"]
RUN dotnet restore "WebApp_UnderTheHood/WebApp_UnderTheHood.csproj"
COPY . .
WORKDIR "/src/WebApp_UnderTheHood"
RUN dotnet build "WebApp_UnderTheHood.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "WebApp_UnderTheHood.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApp_UnderTheHood.dll"]
