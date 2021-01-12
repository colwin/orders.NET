FROM microsoft/dotnet:latest

COPY . /app

WORKDIR /app

RUN ["dotnet", "restore"]
RUN apt-get update && apt-get install -y tcpdump jq
RUN ["dotnet", "build"]

EXPOSE 80/tcp

RUN [ "curl", "-LO", "https://github.com/DataDog/dd-trace-dotnet/releases/download/v1.21.1/datadog-dotnet-apm_1.21.1_amd64.deb",  "&&", "dpkg", "-i", "./datadog-dotnet-apm_1.21.1_amd64.deb"]

ENV CORECLR_ENABLE_PROFILING=1
ENV CORECLR_PROFILER={846F5F1C-F9AE-4B07-969E-05C26BC060D8}
ENV CORECLR_PROFILER_PATH=/opt/datadog/Datadog.Trace.ClrProfiler.Native.so
ENV DD_INTEGRATIONS=/opt/datadog/integrations.json
ENV DD_DOTNET_TRACER_HOME=/opt/datadog


ENTRYPOINT ["dotnet", "run", "--server.urls", "http://0.0.0.0:80"]
