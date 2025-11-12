# Build stage
FROM mcr.microsoft.com/dotnet/sdk:10.0-noble-aot AS build
WORKDIR /source

# Copy project files
COPY ["src/OrderProcessor/OrderProcessor.csproj", "src/OrderProcessor/"]
COPY ["src/eShop.ServiceDefaults/eShop.ServiceDefaults.csproj", "src/eShop.ServiceDefaults/"]
COPY ["src/EventBusRabbitMQ/EventBusRabbitMQ.csproj", "src/EventBusRabbitMQ/"]
COPY ["src/EventBus/EventBus.csproj", "src/EventBus/"]
COPY ["Directory.Packages.props", "./"]
COPY ["Directory.Build.props", "./"]
COPY ["Directory.Build.targets", "./"]

# Restore dependencies
RUN dotnet restore "src/OrderProcessor/OrderProcessor.csproj"

# Copy source code
COPY src/ src/

# Publish application
WORKDIR /source/src/OrderProcessor
RUN dotnet publish "OrderProcessor.csproj" -c Release -o /app /p:EnableAotPublishing=true \
    && rm /app/*.dbg

# Runtime stage
FROM mcr.microsoft.com/dotnet/runtime-deps:10.0-noble-chiseled
WORKDIR /app
COPY --from=build /app .
ENTRYPOINT ["./OrderProcessor"]
