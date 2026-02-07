# docker-run

Minimal Docker-based data pipeline setup for ingesting NYC Taxi data into Postgres.

## Structure

- [pipeline/](pipeline/) — ingestion code and Docker assets
- [test/](test/) — simple file listing script and sample files

## Prerequisites

- Docker + Docker Compose
- Python 3.13 (for local runs)

## Quick Start (Docker Compose)

From [pipeline/docker-compose.yaml](pipeline/docker-compose.yaml):

```bash
docker compose -f pipeline/docker-compose.yaml up -d
```

## Ingest Data (Docker)

Build the image (Dockerfile in [pipeline/Dockerfile](pipeline/Dockerfile)):

```bash
docker build -t taxi_ingest:v001 ./pipeline
```

Run the ingest container (example):

```bash
docker run -it --rm \
  --network=pipeline_default \
  taxi_ingest:v001 \
  --pg-user=root \
  --pg-pass=root \
  --pg-host=pgdatabase \
  --pg-port=5432 \
  --pg-db=ny_taxi \
  --target-table=yellow_taxi_trips_2021_1 \
  --chunksize=100000
```

## Local Run (Python)

```bash
python pipeline/ingest_data.py --pg-host=localhost
```

## Notes

- Ingestion logic: [`ingest_data`](pipeline/ingest_data.py) in [pipeline/ingest_data.py](pipeline/ingest_data.py)
- Docker commands example: [pipeline/how-to-run.sh](pipeline/how-to-run.sh)