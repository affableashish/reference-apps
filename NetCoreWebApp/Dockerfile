#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

#AshishK Notes: aspnet:6.0 contains runtime components, sdk contains a lot more.
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["NetCoreWebApp/NetCoreWebApp.csproj", "NetCoreWebApp/"]
RUN dotnet restore "NetCoreWebApp/NetCoreWebApp.csproj"
COPY . .
WORKDIR "/src/NetCoreWebApp"
RUN dotnet build "NetCoreWebApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "NetCoreWebApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "NetCoreWebApp.dll"]