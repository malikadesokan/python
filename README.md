# Workforce Data Pipeline with Apache Airflow, Astro CLI, Google Cloud Storage & BigQuery

## Overview

This project demonstrates a simple end-to-end data engineering pipeline built using Apache Airflow (via Astro CLI), Docker, and Google Cloud Platform.

The pipeline automatically:

1. Generates a sample workforce dataset using Python.
2. Saves the dataset locally as a timestamped CSV file.
3. Uploads all CSV files from the local data directory to a Google Cloud Storage (GCS) bucket.
4. Loads the uploaded data into a BigQuery table for analysis.

The objective of this project is to gain hands-on experience with modern data engineering tools including Apache Airflow, Astro CLI, Docker, Google Cloud Storage, BigQuery, and Infrastructure as Code concepts.

---

# Architecture

```
                +---------------------+
                | Python Data Script  |
                | Generates CSV Files |
                +----------+----------+
                           |
                           |
                           v
              Local Airflow Data Folder
           (/usr/local/airflow/dags/data)
                           |
                           |
                           v
      LocalFilesystemToGCSOperator
                           |
                           |
                           v
          Google Cloud Storage (GCS)
                           |
                           |
                           v
          GCSToBigQueryOperator
                           |
                           |
                           v
                 BigQuery Table
```

---

# Technologies Used

| Technology | Purpose |
|------------|---------|
| Python | Generate sample workforce data |
| Pandas | Data manipulation |
| Apache Airflow | Workflow orchestration |
| Astro CLI | Local Airflow development |
| Docker | Containerised environment |
| Google Cloud Storage | Store raw CSV files |
| BigQuery | Data warehouse |
| Terraform | Provision cloud resources |
| Git & GitHub | Version control |

---

# Project Structure

```
.
├── dags/
│   ├── workforce_pipeline.py
│   └── data/
│       ├── workforce_20260716_150210.csv
│       ├── workforce_20260716_160815.csv
│       └── ...
│
├── include/
├── plugins/
├── config.py
├── Dockerfile
├── requirements.txt
├── terraform/
└── README.md
```

---

# Workflow

## Step 1

Generate a workforce dataset using Python.

The dataset is saved with a timestamp to ensure that each execution creates a new file.

Example

```
workforce_20260716_181500.csv
```

---

## Step 2

Airflow searches the local data directory and uploads every CSV file to Google Cloud Storage.

Destination example

```
gs://ade-demo-bucket-123456/raw/
```

---

## Step 3

Airflow loads the uploaded CSV files into BigQuery.

The resulting table can then be queried using SQL or visualised in BI tools such as Power BI or Looker Studio.

---

# DAG Overview

The DAG consists of two main tasks.

```
upload_csv
      |
      |
      v
load_gcs_data_to_bq
```

### upload_csv

Uses

```
LocalFilesystemToGCSOperator
```

Purpose

- Reads CSV files from the local directory
- Uploads them into GCS

---

### load_gcs_data_to_bq

Uses

```
GCSToBigQueryOperator
```

Purpose

- Reads uploaded files
- Loads data into BigQuery
- Creates the destination table if required

---

# Running the Project

## Start Astro

```bash
astro dev start
```

---

## List DAGs

```bash
astro dev run dags list
```

---

## Test DAG

```bash
astro dev run dags test workforce_pipeline
```

---

## View Logs

```bash
astro dev logs scheduler
```

---

## Open Airflow

```
http://localhost:8080
```

---

# Configuration

Project configuration is stored in

```
config.py
```

Example

```python
project_id = "your-project-id"

gcs_bucket_name = "your-bucket"

data_source_path = "/usr/local/airflow/dags/data"

dataset_id = "ade_demo_bigquery"

table_name = "practice_table"
```

---

# Sample Output

Generated files

```
workforce_20260716_190020.csv
workforce_20260716_190150.csv
workforce_20260716_190400.csv
```

Uploaded objects

```
gs://ade-demo-bucket-123456/raw/workforce_20260716_190020.csv

gs://ade-demo-bucket-123456/raw/workforce_20260716_190150.csv
```

BigQuery

```
practice_table
```

---

# Key Learning Outcomes

Through this project I learned how to:

- Build an Apache Airflow DAG using Astro CLI
- Containerise an Airflow environment using Docker
- Upload local files into Google Cloud Storage
- Load data from GCS into BigQuery
- Configure Airflow providers for Google Cloud
- Debug Airflow task failures using scheduler logs
- Work with Airflow Operators
- Use timestamped file generation to avoid overwriting files
- Organise Airflow projects using configuration files

---

# Future Improvements

Potential enhancements include:

- Add data quality validation using Great Expectations
- Implement incremental loading into BigQuery
- Archive processed files instead of re-uploading
- Introduce dbt for data transformation
- Trigger the pipeline using Cloud Composer
- Add monitoring and alerting
- Deploy using CI/CD with GitHub Actions
- Parameterise the pipeline using Airflow Variables
- Store secrets in Secret Manager

---

# Author

**Abdulmalik Ademola Adesokan**

Data Analyst | Aspiring Data Engineer

GitHub:
(Your GitHub URL)

LinkedIn:
(Your LinkedIn URL)
