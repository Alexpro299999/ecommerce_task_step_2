# 🚀Ecommerce Data Pipeline - Step 2: Airflow Migration

This repository contains Step 2 of the e-commerce data pipeline project. The primary goal of this step is to abandon the monolithic execution model and orchestrate the pipeline using Apache Airflow. We also shift from passing data in-memory (RAM) to passing intermediate states via the file system.

## 🎯Objectives

1. **Infrastructure Update**: Added base Airflow services (`airflow-webserver`, `airflow-scheduler`, `airflow-init`) to `docker-compose.yml`. The environment is configured to use `LocalExecutor` to allow parallel task execution.
2. **Deprecate the Monolith**: Removed the old `main.py` and `main_pipeline.py` files. All orchestration logic now lives strictly inside the Airflow DAG.
3. **DAG Implementation**: Created the `ecommerce_daily_report` DAG. The pipeline is broken down into simple tasks that invoke the underlying Python business logic:
   * `extract_files_task`
   * `extract_db_task`
   * `transform_task`
   * `load_task`

## 📋Acceptance Criteria

* [ ]  The `ecommerce_daily_report` DAG is visible in the Airflow UI.
* [ ]  When triggered, the DAG executes the pipeline successfully.
* [ ]  The final report is saved into the `reports/` folder.

## 🛠️How to Run

1. Initialize and start the Docker containers (PostgreSQL database + Airflow services):

```bash
   docker-compose up -d
```

2. Open the Airflow UI by navigating to `http://localhost:8080` (default credentials: `airflow` / `airflow`).
3. Find the `ecommerce_daily_report` DAG, unpause it, and trigger a run.
4. Monitor the task execution. Once complete, check the `reports/` directory for the output CSV file.
