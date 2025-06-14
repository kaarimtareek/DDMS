FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 6063
ENV ASPNETCORE_URLS=http://0.0.0.0:6063

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Documents.Service/Documents.Service.csproj", "Documents.Service/"]
RUN dotnet restore "Documents.Service/Documents.Service.csproj"
COPY . .
WORKDIR "/src/Documents.Service"
RUN dotnet build "./Documents.Service.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Documents.Service.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Documents.Service.dll"]
