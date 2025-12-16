FROM clickhouse/clickhouse-server:24.3

# Copy ClickHouse configuration
COPY config.xml /etc/clickhouse-server/config.xml
COPY users.xml /etc/clickhouse-server/users.xml
COPY cluster.xml /etc/clickhouse-server/config.d/cluster.xml
COPY custom-function.xml /etc/clickhouse-server/custom-function.xml

# Download histogram-quantile binary for custom functions
RUN mkdir -p /var/lib/clickhouse/user_scripts && \
    apt-get update && apt-get install -y wget && \
    wget -O /tmp/histogram-quantile.tar.gz "https://github.com/SigNoz/signoz/releases/download/histogram-quantile%2Fv0.0.1/histogram-quantile_linux_amd64.tar.gz" && \
    tar -xzf /tmp/histogram-quantile.tar.gz -C /tmp && \
    mv /tmp/histogram-quantile /var/lib/clickhouse/user_scripts/histogramQuantile && \
    chmod +x /var/lib/clickhouse/user_scripts/histogramQuantile && \
    rm /tmp/histogram-quantile.tar.gz && \
    apt-get clean

ENV CLICKHOUSE_SKIP_USER_SETUP=1
