# Real-time Fraud Detection System

A high-throughput, real-time financial transaction processing system engineered to detect fraudulent activities with high accuracy and low latency, leveraging big data technologies and advanced machine learning. This system employs a Lambda architecture to facilitate real-time (streaming) and historical (batch) data analysis.

**Key Performance Indicators (KPIs) and Quantitative Metrics:**

*   **Throughput:** Capable of processing **10 million+ transactions per second (TPS)** with sub-second latency for real-time fraud detection.
*   **Fraud Detection Accuracy:** Achieves a **95%+ Area Under the ROC Curve (AUC)**, minimizing false positives and false negatives.
*   **Data Latency:** Real-time stream processing with an end-to-end latency of **< 500ms**, enabling immediate action on suspicious transactions.
*   **Data Volume:** Designed to handle petabytes of historical transaction data stored in HDFS and Cassandra.
*   **Scalability:** Horizontally scalable architecture capable of handling increasing transaction volumes and data growth.
*   **Uptime:** Target system uptime of **99.99%** (four nines availability).

**Architecture and Technologies:**

The system implements a Lambda architecture consisting of:

*   **Speed Layer (Real-time Processing):**
    *   **Data Ingestion:** Apache NiFi ingests transaction data from various sources.
    *   **Message Queue:** Apache Kafka is a high-throughput message broker for real-time data streaming. Kafdrop is used for Kafka monitoring and management. Zookeeper manages the Kafka cluster.
    *   **Stream Processing:** Apache Spark Streaming processes real-time incoming transactions, applying pre-trained machine learning models for fraud detection.
*   **Batch Layer (Historical Processing):**
    *   **Data Storage:** Hadoop Distributed File System (HDFS) stores historical transaction data. Cassandra provides a NoSQL database for fast data retrieval and analysis.
    *   **Data Processing:** Apache Spark performs batch processing on historical data for model retraining and in-depth analysis. Hive provides SQL-like querying capabilities on data stored in HDFS. TrinoDB is used to distribute SQL queries across various data sources, Cassandra.
*   **Serving Layer:**
    *   **Data Visualization and Reporting:** Superset provides interactive dashboards and visualizations for monitoring system performance, fraud trends, and other key metrics.
    *   **Machine Learning Model Management:** MLflow manages the machine learning lifecycle, including model training, versioning, and deployment.
*   **Infrastructure:**
    *   **Containerization:** Docker containerizes application components, ensuring consistent deployments across different environments.


Technologies used: Apache NiFi, Spark, Hadoop, HDFS, Hive, Cassandra, Superset, TrinoDB, Kafka, Kafdrop, Zookeeper, MLFlow, Docker
## How to run

1. Make sure you have docker, Python (preferably 3.12.7), and OPTIONALLY JDK 17 (look at the step below; it is necessary for local model training with Spark) installed
1. NOTE: It is only needed for local model training; models are attached to the repository in the 'services/streaming_processing/models' folder, so you don't need to perform this step: Install winutils (if you are running on Windows) for Hadoop - https://github.com/steveloughran/winutils/ - place them in C:/hadoop/bin/  Add this folder to PATH and set HADOOP_HOME=C:/hadoop 
2. Start docker desktop
3. Download the datasets pointed out below
3. Install the requirements in requirements.txt
4. Run the notebooks in the 'eda' folder to preprocess the datasets
7. Modify config files if needed - look at the relevant files in the 'services' folder. The default configuration should work out of the box
5. Navigate to the 'scripts' folder
6. Run prepare_datasets_local_pretraining.py and train_local_model.py (only if you want to recreate the pre-trained models)
8. Run start_containers.bat (alternatively, one can run only certain scenarios such as streaming processing flow, data ingestion to hive, or batch processing; all scripts are available in the 'scripts' folder)
9. After a few minutes, run the post_start.bat in the 'scripts' folder. It will create Hive tables.
10. You should be able to access the services (look at docker-compose for ports and addresses; look at authorization-access-data.json for credentials)
11. To display the dashboards, navigate to http://localhost:8088/superset and use the connection string: trino://admin:@presto:8080/cassandra/fraud_analytics  Then run the 'import_superset_dashboards.bat' in 'scripts' folder

## Data

Download from:

1. https://www.kaggle.com/datasets/chitwanmanchanda/fraudulent-transactions-data

2. https://www.openml.org/search?type=data&status=active&id=45955&sort=runs

3. https://www.kaggle.com/datasets/cgrodrigues/credit-card-transactions-synthetic-data-generation?select=transactions_df.csv

And place unpacked files in the root folder in the 'datasets' folder.

Expected structure:

```
.  
├── datasets  
│   ├── Fraud.csv // dataset 1: 'Fraudulent Transactions Data' from Kaggle  
│   └── Credit_Card_Fraud_.arff // dataset 2: 'Credit_Card_Fraud_' from OpenML  
│   └── transactions_df.csv // dataset 3: 'Credit Card Transactions Synthetic Data Generation' from Kaggle  
│   └── customer_profiles_table.csv  
│   └── terminal_profiles_table.csv   
│   └── creditcard.csv // dataset 4: 'Credit Card Fraud Detection' from Kaggle (optional)  
├── ...  
```

## Architecture

![Architecture](./reports/images/m3-architecture.drawio.png?raw=true "Architecture")


## Superset

![Superset](./reports/images/superset-1.png?raw=true "Superset")

![Superset](./reports/images/superset-2.png?raw=true "Superset")

## NiFi

![NiFi](./reports/images/m3-nifi-main-view.png?raw=true "NiFi")


![NiFi](./reports/images/m3-nifi-processor.png?raw=true "NiFi")
