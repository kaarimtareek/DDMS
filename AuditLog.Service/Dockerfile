FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 6062
ENV ASPNETCORE_URLS=http://0.0.0.0:6062

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["AuditLog.Service/AuditLog.Service.csproj", "AuditLog.Service/"]
RUN dotnet restore "AuditLog.Service/AuditLog.Service.csproj"
COPY . .
WORKDIR "/src/AuditLog.Service"
RUN dotnet build "./AuditLog.Service.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./AuditLog.Service.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AuditLog.Service.dll"]
