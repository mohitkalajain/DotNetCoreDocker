#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
RUN apt-get update && apt-get install -y apt-utils libgdiplus libc6-dev

WORKDIR /app
EXPOSE 80
#EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/TestApplication/TestApplication.csproj", "src/TestApplication/"]
RUN dotnet restore "src/TestApplication/TestApplication.csproj"
COPY . .
WORKDIR "/src"
RUN dotnet build "src/TestApplication/TestApplication.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "src/TestApplication/TestApplication.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TestApplication.dll"]