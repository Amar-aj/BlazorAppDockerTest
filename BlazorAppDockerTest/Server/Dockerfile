#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["BlazorAppDockerTest/Server/BlazorAppDockerTest.Server.csproj", "BlazorAppDockerTest/Server/"]
COPY ["BlazorAppDockerTest/Client/BlazorAppDockerTest.Client.csproj", "BlazorAppDockerTest/Client/"]
COPY ["BlazorAppDockerTest/Shared/BlazorAppDockerTest.Shared.csproj", "BlazorAppDockerTest/Shared/"]
RUN dotnet restore "BlazorAppDockerTest/Server/BlazorAppDockerTest.Server.csproj"
COPY . .
WORKDIR "/src/BlazorAppDockerTest/Server"
RUN dotnet build "BlazorAppDockerTest.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorAppDockerTest.Server.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorAppDockerTest.Server.dll"]