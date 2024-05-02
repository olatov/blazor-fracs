FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["Fracs.Wasm/Fracs.Wasm.csproj", "Fracs.Wasm/"]
RUN dotnet restore "Fracs.Wasm/Fracs.Wasm.csproj"
COPY . .
WORKDIR "/src/Fracs.Wasm"
RUN dotnet build "Fracs.Wasm.csproj" -c "$configuration" -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "Fracs.Wasm.csproj" -c "$configuration" -o /app/publish /p:UseAppHost=false

FROM rtsp/lighttpd:latest AS final
WORKDIR /var/www/html/
COPY --from=publish /app/publish/wwwroot .
