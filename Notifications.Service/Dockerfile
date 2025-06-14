FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 6065
ENV ASPNETCORE_URLS=http://0.0.0.0:6065

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Notifications.Service/Notifications.Service.csproj", "Notifications.Service/"]
RUN dotnet restore "Notifications.Service/Notifications.Service.csproj"
COPY . .
WORKDIR "/src/Notifications.Service"
RUN dotnet build "./Notifications.Service.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Notifications.Service.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Notifications.Service.dll"]
