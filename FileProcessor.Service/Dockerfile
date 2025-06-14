FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 6064
ENV ASPNETCORE_URLS=http://0.0.0.0:6064

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["FileProcessor.Service/FileProcessor.Service.csproj", "FileProcessor.Service/"]
RUN dotnet restore "FileProcessor.Service/FileProcessor.Service.csproj"
COPY . .
WORKDIR "/src/FileProcessor.Service"
RUN dotnet build "./FileProcessor.Service.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./FileProcessor.Service.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "FileProcessor.Service.dll"]
