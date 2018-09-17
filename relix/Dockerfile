FROM microsoft/dotnet:2.1.4-aspnetcore-runtime-bionic AS base
WORKDIR /app
EXPOSE 80

ENV CORECLR_ENABLE_PROFILING="1" \
ASPNETCORE_ENVIRONMENT="production" \
CORECLR_PROFILER="{36032161-FFC0-4B61-B559-F6C5D41BAE5A}" \
CORECLR_NEWRELIC_HOME="/usr/local/newrelic-netcore20-agent" \
CORECLR_PROFILER_PATH="/usr/local/newrelic-netcore20-agent/libNewRelicProfiler.so" \
NEW_RELIC_LICENSE_KEY="add-key-here" \
NEW_RELIC_APP_NAME="relix"

ARG NewRelic="relix/newrelic/"
COPY $NewRelic ./newrelic
#
RUN dpkg -i ./newrelic/newrelic-netcore20-agent_8.6.45.0_amd64.deb



FROM microsoft/dotnet:2.1.402-sdk-bionic AS build
WORKDIR /src
COPY ["relix/relix.csproj", "relix/"]
RUN dotnet restore "relix/relix.csproj"
COPY . .
WORKDIR "/src/relix"
RUN dotnet build "relix.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "relix.csproj" -c Release -o /app



FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "relix.dll"]