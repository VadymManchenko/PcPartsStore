FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["pc-parts-store/pc-parts-store.csproj", "pc-parts-store/"]
RUN dotnet restore "pc-parts-store/pc-parts-store.csproj"
COPY . .
WORKDIR "/src/pc-parts-store"
RUN dotnet build "pc-parts-store.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "pc-parts-store.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "pc-parts-store.dll"]
