FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
ENV DOTNET_CLI_TELEMETRY_OPTOUT 1
ENV DOTNET_SKIP_FIRST_TIME_EXPERIENCE 1
ENV DOTNET_NOLOGO 1
    
FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
ENV DOTNET_CLI_TELEMETRY_OPTOUT 1
ENV DOTNET_SKIP_FIRST_TIME_EXPERIENCE 1
ENV DOTNET_NOLOGO 1
WORKDIR /src

COPY ["MyWebApi.csproj", "MyWebApi/"]
RUN dotnet restore "MyWebApi/MyWebApi.csproj"

COPY . MyWebApi/

WORKDIR /src/MyWebApi

RUN dotnet build "MyWebApi.csproj" -c Release --no-restore -o /app/build

FROM build AS publish
ENV DOTNET_CLI_TELEMETRY_OPTOUT 1
ENV DOTNET_SKIP_FIRST_TIME_EXPERIENCE 1
ENV DOTNET_NOLOGO 1
RUN dotnet publish "MyWebApi.csproj" -c Release --no-restore -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MyWebApi.dll"]