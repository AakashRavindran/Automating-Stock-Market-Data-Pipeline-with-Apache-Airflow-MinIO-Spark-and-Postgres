Overview
========

This project retrieves stock prices from the Yahoo Finance website using an API, processes and stores this data, and finally loads it into a data warehouse for analytics and visualization. Additionally, a notification is sent to Slack whenever a task is successfully completed

This project is inspired by a course I took on Udemy, taught by Marc Lamberti. The project helped me understand how to manage and automate data pipelines using industry-standard tools.

Project Contents
================

The astro project contains the following files and folders:

- dags: This folder contains the Python files for your Airflow DAGs. By default, this directory includes one example DAG:
    - `example_astronauts`: This DAG shows a simple ETL pipeline example that queries the list of astronauts currently in space from the Open Notify API and prints a statement for each astronaut. The DAG uses the TaskFlow API to define tasks in Python, and dynamic task mapping to dynamically print a statement for each astronaut. For more on how this DAG works, see our [Getting started tutorial](https://www.astronomer.io/docs/learn/get-started-with-airflow).
- Dockerfile: This file contains a versioned Astro Runtime Docker image that provides a differentiated Airflow experience. If you want to execute other commands or overrides at runtime, specify them here.
- include: This folder contains any additional files that you want to include as part of your project. It is empty by default.
- packages.txt: Install OS-level packages needed for your project by adding them to this file. It is empty by default.
- requirements.txt: Install Python packages needed for your project by adding them to this file. It is empty by default.
- plugins: Add custom or community plugins for your project to this file. It is empty by default.
- airflow_settings.yaml: Use this local-only file to specify Airflow Connections, Variables, and Pools instead of entering them in the Airflow UI as you develop DAGs in this project.

Deploy Your Project Locally
===========================

1. Start Airflow on your local machine by running 'astro dev start'.

This command will spin up 4 Docker containers on your machine, each for a different Airflow component:

- Postgres: Airflow's Metadata Database
- Webserver: The Airflow component responsible for rendering the Airflow UI
- Scheduler: The Airflow component responsible for monitoring and triggering tasks
- Triggerer: The Airflow component responsible for triggering deferred tasks

2. Verify that all 4 Docker containers were created by running 'docker ps'.

Note: Running 'astro dev start' will start your project with the Airflow Webserver exposed at port 8080 and Postgres exposed at port 5432. If you already have either of those ports allocated, you can either [stop your existing Docker containers or change the port](https://www.astronomer.io/docs/astro/cli/troubleshoot-locally#ports-are-not-available-for-my-local-airflow-webserver).

3. Access the Airflow UI for your local Airflow project. To do so, go to http://localhost:8080/ and log in with 'admin' for both your Username and Password.

You should also be able to access your Postgres Database at 'localhost:5432/postgres'.

Deploy Your Project to Astronomer
=================================

If you have an Astronomer account, pushing code to a Deployment on Astronomer is simple. For deploying instructions, refer to Astronomer documentation: https://www.astronomer.io/docs/astro/deploy-code/

Contact
=======

The Astronomer CLI is maintained with love by the Astronomer team. To report a bug or suggest a change, reach out to our support.


Step-by-Step Breakdown
=======================

1. Checking if Yahoo API is Available
The process starts by checking if Yahoo Finance API is available using the is_api_available() function. If the API is online, the data retrieval process starts.

2. Fetching Stock Prices
Once we ensure the API is available, the next step is to use the fetch_stock_prices() function to pull stock price data. The prices are retrieved from the Yahoo Finance website for specific stocks (which can be configured).

3. Storing Data in MinIO
After fetching the stock prices, they are stored in MinIO, an S3-compatible object storage. This ensures we have a reliable and scalable storage solution that can handle large amounts of data.

4. Data Transformation with Spark
Once the stock data is stored, Apache Spark comes into play. We use Spark to format the data and perform any necessary transformations before loading it into Postgres.

5. Loading Data to Postgres
After processing, the formatted stock prices are loaded into Postgres for further analysis or visualization.

6. Notifications via Slack
Whenever the pipeline finishes a task, we notify a Slack channel using Airflow’s Slack integration. This helps keep track of the workflow and identify if anything goes wrong.

7.Monitoring with Metabase
To visualize the stock price data, I’ve connected the Postgres database to Metabase, an open-source business intelligence tool. Metabase allows you to create dashboards and get insights from the stock price data easily.


Credits to Marc Lamberti’s course on Udemy for inspiring this project
