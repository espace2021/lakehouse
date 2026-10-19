FROM apache/airflow:3.1.0-python3.12
# dbt dans un venv séparé pour éviter les conflits de dépendances avec Airflow
USER airflow
RUN python -m venv /home/airflow/dbt-venv \
 && /home/airflow/dbt-venv/bin/pip install --no-cache-dir "dbt-core~=1.10" "dbt-trino~=1.9"
RUN pip install --no-cache-dir "apache-airflow==${AIRFLOW_VERSION}" apache-airflow-providers-trino
RUN python -m venv /home/airflow/dbt-venv \
 && /home/airflow/dbt-venv/bin/pip install --no-cache-dir "dbt-core~=1.10" "dbt-trino~=1.9" "dbt-spark[PyHive]~=1.9"
