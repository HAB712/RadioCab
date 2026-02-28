# -------------------------------
# Build Stage
# -------------------------------
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy csproj first (for caching)
COPY ["RadioCab/RadioCab.csproj", "RadioCab/"]

# Restore dependencies
RUN dotnet restore "RadioCab/RadioCab.csproj"

# Copy everything else
COPY . .

# Publish app
RUN dotnet publish "RadioCab/RadioCab.csproj" \
    -c Release \
    -o /app/publish \
    /p:UseAppHost=false

# -------------------------------
# Runtime Stage
# -------------------------------
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app

# Copy published output
COPY --from=build /app/publish .

# Railway required port
ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

# Start app
ENTRYPOINT ["dotnet", "RadioCab.dll"]
