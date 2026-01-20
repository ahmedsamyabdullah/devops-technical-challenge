# Stage 1: Build Stage ( Using SDK image )
# -----------------------------------------
FROM mcr.microsoft.com/dotnet/sdk:10.0-alpine AS build
WORKDIR /src
# First COPY .csproj only to speed Building (Cached layers)
COPY ["SimpleBackend.csproj", "./"]         
RUN dotnet restore "SimpleBackend.csproj"
COPY . . 
RUN dotnet publish "SimpleBackend.csproj" -c Release -o /app/publish /p:UseAppHost=false
# ---------------------------------------------------------------------------------------
# Stage 2: Runtime Stage
# ----------------------
FROM mcr.microsoft.com/dotnet/aspnet:10.0-alpine AS final
WORKDIR /app
# For Security: Run as a non-root user  (Creating User = appuser)
RUN adduser --disabled-password --home /app --gecos '' appuser && chown -R appuser /app 
USER appuser
EXPOSE 8080
# Copy the published artifacts from the build stage
COPY --from=build /app/publish .     
ENTRYPOINT ["dotnet", "SimpleBackend.dll"]
