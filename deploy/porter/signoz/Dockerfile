FROM signoz/signoz:v0.104.0

# Copy Prometheus configuration
COPY prometheus.yml /root/config/prometheus.yml

# Copy dashboards
COPY dashboards /root/config/dashboards

ENV SIGNOZ_ALERTMANAGER_PROVIDER=signoz
ENV STORAGE=clickhouse
ENV GODEBUG=netdns=go
ENV TELEMETRY_ENABLED=true
ENV DEPLOYMENT_TYPE=porter

ENTRYPOINT ["./signoz", "server", "--config=/root/config/prometheus.yml"]
