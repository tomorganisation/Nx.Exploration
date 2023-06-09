#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["Nx.Exploration.API/Nx.Exploration.API.csproj", "Nx.Exploration.API/"]
RUN dotnet restore "Nx.Exploration.API/Nx.Exploration.API.csproj"
COPY . .
WORKDIR "/src/Nx.Exploration.API"
RUN dotnet build "Nx.Exploration.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Nx.Exploration.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Nx.Exploration.API.dll"]