FROM mcr.microsoft.com/dotnet/sdk:8.0 AS base
WORKDIR /app
EXPOSE 8075
EXPOSE 8080

ENV ASPNETCORE_URLS=http://+:8075

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["/src/ApiPfx/ApiPfx.csproj", "./"]
RUN dotnet restore "ApiPfx.csproj"

COPY . .
WORKDIR "/src/."
RUN dotnet build "ApiPfx.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "ApiPfx.csproj" -c $configuration -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ApiPfx.dll"]
