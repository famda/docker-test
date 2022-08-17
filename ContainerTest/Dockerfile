FROM mcr.microsoft.com/dotnet/sdk:6.0-alpine AS publish
WORKDIR /src
COPY ["ContainerTest/ContainerTest.csproj", "ContainerTest/"]
RUN dotnet restore "ContainerTest/ContainerTest.csproj" --runtime linux-musl-x64
COPY . .
RUN dotnet publish "ContainerTest/ContainerTest.csproj" -c Release -o /app/publish \
	--no-restore \
	--runtime linux-musl-x64 \
	--self-contained true \
	/p:PublishTrimmed=true \
	/p:PublishSingleFile=true

FROM mcr.microsoft.com/dotnet/runtime-deps:6.0-alpine AS final
WORKDIR /app

EXPOSE 80
COPY --from=publish /app/publish .
ENTRYPOINT ["./ContainerTest"]
