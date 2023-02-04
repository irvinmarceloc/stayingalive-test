#FROM registry.redhat.io/ubi8/dotnet-70  
FROM quay.io/paulchapmanibm/ppc64le/dotnet-70
#FROM dotnet-70 AS build-env
#FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
#USER root
#RUN mkdir /app
#USER root
WORKDIR /app
#COPY --chown=1001 /app /app
EXPOSE 8080
EXPOSE 443

FROM dotnet-70 AS build
#FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY --chown=1001 ["stayingalive/stayingalive.csproj", "stayingalive/"]
RUN dotnet restore "stayingalive/stayingalive.csproj"
COPY --chown=1001 . ./
WORKDIR "/src/stayingalive"
USER root
RUN dotnet build "stayingalive.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "stayingalive.csproj" -c Release -o /app/publish /p:UseAppHost=false

#USER root
FROM dotnet-70 AS final
WORKDIR /app
COPY --chown=1001 --from=publish /app/publish .
ENTRYPOINT ["dotnet", "stayingalive.dll"]

