# Plateforme lakehouse open source (TP)

SeaweedFS (S3) · Iceberg · Nessie · Kafka · Spark 4.1 · Trino · dbt · Airflow · MLflow

Cette plateforme ne contient **aucun cas d'usage** : un projet (par exemple `fintrack-ateliers`) s'y branche avec ses DAG Airflow, son projet dbt et ses jobs Spark.

## 1. Brancher un projet

Placez le projet **à côté** de ce dossier :

```
machine/
├── lakehouse/            ← ce dossier (la plateforme)
└── fintrack-ateliers/    ← le projet branché
```

Le projet est désigné dans `.env` :

```
DAGS_DIR=../fintrack-ateliers/dags
DBT_DIR=../fintrack-ateliers/dbt
JOBS_DIR=../fintrack-ateliers/jobs
```

Pour brancher un autre cours, changez ces trois lignes, puis `docker compose up -d`.

## 2. Démarrer

```bash
docker compose up -d --build          # cœur de la plateforme
docker compose --profile ml up -d     # + MLflow
```

La plateforme crée le réseau Docker `lakehouse-net`, que les projets rejoignent. **Démarrez-la toujours en premier.**

## 3. Les services

| Service | URL |
|---|---|
| Airflow | http://localhost:8080 |
| Kafka UI | http://localhost:8081 |
| Spark UI | http://localhost:8082 |
| Trino | http://localhost:8085 |
| Nessie | http://localhost:19120 |
| MLflow | http://localhost:5000 |
| SeaweedFS (explorateur de fichiers) | http://localhost:8888/buckets/ |
| SeaweedFS (API S3, pour les programmes) | http://localhost:8333 (clé `admin` / `password`) |

Depuis votre poste : Kafka sur `localhost:29092`. Depuis un conteneur du réseau : `kafka:9092`, `nessie:19120`, `seaweedfs:8333`, `trino:8080`.

## 4. Vérifier

```bash
docker compose logs init-buckets
docker compose exec trino trino --execute "CREATE SCHEMA IF NOT EXISTS iceberg.demo; CREATE TABLE IF NOT EXISTS iceberg.demo.t AS SELECT 1 AS id, 'ok' AS status; SELECT * FROM iceberg.demo.t;"
docker compose exec spark-master /opt/spark/bin/spark-submit --version
```

⚠️ `docker compose down -v` efface toutes les données (tables, topics, historique). Utilisez `docker compose down` sans `-v`.

Identifiants de TP dans `.env` et `conf/seaweedfs/s3.json` (à garder synchronisés). Ne pas utiliser en production.
