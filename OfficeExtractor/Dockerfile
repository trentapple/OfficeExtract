# Learn about building .NET container images:
# https://github.com/dotnet/dotnet-docker/blob/main/samples/README.md
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:9.0-alpine AS build
ARG TARGETARCH
WORKDIR /source

# Copy project file and restore as distinct layers
COPY *.csproj .
RUN dotnet restore -a $TARGETARCH

# Copy source code and publish app
COPY . .
RUN dotnet publish -a $TARGETARCH --no-restore -o /app


# Runtime stage
FROM mcr.microsoft.com/dotnet/runtime:9.0-alpine
WORKDIR /app
#COPY --link --from=build /app .
COPY --from=build /app .
USER $APP_UID
ENTRYPOINT ["./dotnetapp"]
