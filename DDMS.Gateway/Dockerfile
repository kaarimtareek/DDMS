FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 6060
#EXPOSE 6061
ENV ASPNETCORE_URLS=http://0.0.0.0:6060

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Development
WORKDIR /src
COPY ["DDMS.Gateway/DDMS.Gateway.csproj", "DDMS.Gateway/"]
RUN dotnet restore "DDMS.Gateway/DDMS.Gateway.csproj"
COPY . .
WORKDIR "/src/DDMS.Gateway"
RUN dotnet build "./DDMS.Gateway.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./DDMS.Gateway.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DDMS.Gateway.dll"]
