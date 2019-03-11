FROM microsoft/dotnet:2.1-aspnetcore-runtime-stretch-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM microsoft/dotnet:2.1-sdk-stretch AS build
WORKDIR /build
COPY ["src/deployr-oidc-web/Deployr.Oidc.Web.csproj", "src/deployr-oidc-web/"]
RUN dotnet restore "src/deployr-oidc-web/Deployr.Oidc.Web.csproj"
COPY . .
WORKDIR "/build/src/deployr-oidc-web/"
RUN dotnet build "Deployr.Oidc.Web.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "Deployr.Oidc.Web.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Deployr.Oidc.Web.dll"]