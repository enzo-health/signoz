FROM signoz/signoz-otel-collector:v0.129.12

# Copy OTEL Collector configuration
COPY otel-collector-config.yaml /etc/otel-collector-config.yaml
COPY otel-collector-opamp-config.yaml /etc/manager-config.yaml

ENV OTEL_RESOURCE_ATTRIBUTES=host.name=signoz-host,os.type=linux
ENV LOW_CARDINAL_EXCEPTION_GROUPING=false

ENTRYPOINT ["/signoz-collector"]
CMD ["--config=/etc/otel-collector-config.yaml", "--manager-config=/etc/manager-config.yaml", "--copy-path=/var/tmp/collector-config.yaml", "--feature-gates=-pkg.translator.prometheus.NormalizeName"]
