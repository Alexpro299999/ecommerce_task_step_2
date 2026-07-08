# Tips for Step 2: Airflow Implementation

Here are some technical details and best practices relevant to this step:

## 1. State Transfer via File System

In a monolithic script, passing Pandas DataFrames between functions in RAM is standard. In Airflow, tasks are isolated and can run on different worker processes. Instead of overloading Airflow's metadata database by passing large datasets through XComs, tasks now write their intermediate outputs to the file system (e.g., temporary CSV/JSON files). The downstream tasks then read these files.

## 2. LocalExecutor vs. SequentialExecutor

By configuring `LocalExecutor` in `docker-compose.yml` (backed by a PostgreSQL metadata database), Airflow can run multiple tasks concurrently. This means `extract_files_task` and `extract_db_task` will run at the same time, optimizing the overall extraction phase.

## 3. "Dumb" Tasks

Airflow is an orchestrator, not a data processing engine. Keep your DAG file clean. Make the tasks in `ecommerce_dag.py` are intentionally "dumb"— have them simply import and trigger the actual extraction, transformation, and loading functions located in the `pipeline/` module.

## 4. Volume Mapping

Since tasks pass data via files, ensure that your shared directories (`/data`, `/reports`, and any temp folders) are properly mounted across all Airflow containers (`webserver`, `scheduler`, and `worker`/local executor) in the `docker-compose.yml`. If a downstream task throws a "File not found" error, volume mapping is usually the culprit.
