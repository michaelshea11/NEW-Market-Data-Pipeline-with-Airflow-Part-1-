# Airflow Mini-Project: Market Volatility DAG (`marketvol`)

This project implements an Apache Airflow data pipeline to extract intra-day stock market data (1-minute intervals) for **AAPL** and **TSLA** using Yahoo Finance, processes the files, and runs analytical queries[cite: 10].

## Project Workflow & Architecture
The pipeline is orchestrated by the `marketvol` DAG and executes the following steps:
1. **`t0` (BashOperator):** Initializes a date-specific temporary directory (e.g., `/tmp/data/YYYY-MM-DD`)[cite: 12].
2. **`t1` & `t2` (PythonOperators):** Download 1-minute interval market data concurrently for AAPL and TSLA via `yfinance`[cite: 10, 12].
3. **`t3` & `t4` (BashOperators):** Move the downloaded CSV files into a designated processing directory[cite: 12].
4. **`t5` (PythonOperator):** Executes custom analytical queries on both dataset files once `t3` and `t4` complete successfully[cite: 10, 12].

## Prerequisites
* Python 3.11+
* Apache Airflow 2.10.2
* Required Python Libraries: `yfinance`, `pandas`[cite: 11]

## Setup & Installation Instructions

1. **Clone the repository / navigate to your project directory:**
   ```bash
   cd ~/springboard_projects/airflow_marketvol_project

2. **Activate your virtual environment and install dependencies:**
    source .venv/bin/activate
    pip install apache-airflow yfinance pandas

3. **Configure AIRFLOW_HOME and place the DAG:**
    Ensure marketvol_dag.py is placed inside your designated dags/ folder.

## How to Run and Verify

1. **Initialize the Airflow database (if not already done):**
    airflow db init

2. **Start the Airflow Scheduler:**
    airflow scheduler

3. **Start the Airflow Webserver (in a separate terminal tab):**
    Start the Airflow Webserver (in a separate terminal tab):

4. **Trigger the Pipeline:**
    Open your browser and navigate to http://localhost:8080.
    Locate the marketvol DAG, toggle it On, and manually trigger a run or let it run on its weekday schedule (6 PM Mon–Fri)