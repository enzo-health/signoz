# Deploying SigNoz on Porter

SigNoz requires multiple services. Deploy them in this order:

## 1. Deploy ZooKeeper
```bash
cd deploy/porter/zookeeper
porter create --app signoz-zookeeper -f porter.yaml --project 15255 --cluster 4709
```

## 2. Deploy ClickHouse
```bash
cd deploy/porter/clickhouse
# First build and push the custom image
docker build -t signoz-clickhouse .
docker tag signoz-clickhouse:latest <your-ecr>/signoz-clickhouse:latest
docker push <your-ecr>/signoz-clickhouse:latest

porter create --app signoz-clickhouse -f porter.yaml --project 15255 --cluster 4709
```

## 3. Run Schema Migrations
```bash
porter job run signoz-clickhouse --job schema-migrator --project 15255 --cluster 4709
```

## 4. Deploy SigNoz Backend
```bash
cd deploy/porter/signoz
porter create --app signoz-backend -f porter.yaml --project 15255 --cluster 4709
```

## 5. Deploy OTEL Collector
```bash
cd deploy/porter/otel-collector
porter create --app signoz-otel-collector -f porter.yaml --project 15255 --cluster 4709
```

## 6. Update Frontend
Update the frontend to point to the signoz-backend URL.

## Service URLs
- SigNoz UI: https://signoz-backend-xxxxx.onporter.run
- OTEL Collector (HTTP): https://signoz-otel-collector-xxxxx.onporter.run:4318
- OTEL Collector (gRPC): signoz-otel-collector-xxxxx.onporter.run:4317

## Environment Variables Needed
For services to communicate, set these env vars:

### signoz-backend
- `SIGNOZ_TELEMETRYSTORE_CLICKHOUSE_DSN`: tcp://signoz-clickhouse:9000

### signoz-otel-collector
- ClickHouse DSN in config files need to point to signoz-clickhouse

## Notes
- Porter apps communicate via private networking using app names
- Persistent storage uses EFS (AWS Elastic File System)
- ClickHouse needs ~4GB RAM minimum for production
