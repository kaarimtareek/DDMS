FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 6061
ENV ASPNETCORE_URLS=http://0.0.0.0:6061

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Approval.Service/Approval.Service.csproj", "Approval.Service/"]
RUN dotnet restore "Approval.Service/Approval.Service.csproj"
COPY . .
WORKDIR "/src/Approval.Service"
RUN dotnet build "./Approval.Service.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Approval.Service.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Approval.Service.dll"]
