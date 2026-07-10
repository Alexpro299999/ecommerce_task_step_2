# 🚀Ecommerce Data Pipeline - Step 2: Airflow Migration

This repository contains Step 2 of the e-commerce data pipeline project. The primary goal of this step is to abandon the monolithic execution model and orchestrate the pipeline using Apache Airflow. We also shift from passing data in-memory (RAM) to passing intermediate states via the file system.

## 🎯Objectives

1. **Infrastructure Update**: Add base Airflow services (`airflow-webserver`, `airflow-scheduler`, `airflow-init`) to `docker-compose.yml`. Configure the environment to use `LocalExecutor` to allow parallel task execution.
2. **Deprecate the Monolith**: Remove the old `main.py` and `main_pipeline.py` files. All orchestration logic should be located inside the Airflow DAG.
3. **DAG Implementation**: Create the `ecommerce_daily_report` DAG. The pipeline should be broken down into simple tasks that invoke the underlying Python business logic:
   * `extract_files_task`
   * `extract_db_task`
   * `transform_task`
   * `load_task`

## 📋Acceptance Criteria

* [ ]  Each task should be implemented in its own feature branch (determine the number of features and branches yourself).
* [ ]  The `ecommerce_daily_report` DAG should be visible in the Airflow UI.
* [ ]  When triggered, the DAG should orchestrate the pipeline.
* [ ]  The final report should be saved into the `reports/` folder.

## 🛠️How to Run

1. Initialize and start the Docker containers (PostgreSQL database + Airflow services):

```bash
   docker-compose up -d
```

2. Open the Airflow UI by navigating to `http://localhost:8080` (default credentials: `airflow` / `airflow`).
3. Find the `ecommerce_daily_report` DAG, unpause it, and trigger a run.
4. Monitor the task execution. Once complete, check the `reports/` directory for the output CSV file.
