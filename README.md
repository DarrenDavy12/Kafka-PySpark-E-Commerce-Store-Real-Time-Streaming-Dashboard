# 🚀 E-commerce Real-Time Streaming Analytics Dashboard


## 🎯 Business Case

We have a plain e-commerce platform that needs to monitor user clickstream events as they happen. The goal is to gain real-time insights into user engagement, identify popular products, and quickly detect any anomalies (e.g., a sudden spike in errors or unusual activity). This dashboard provides that live monitoring capability.

## 🏗️ Architecture & Tech Stack

The project is built with the following components:

* **Data Ingestion:** Apache Kafka (using `kafka-python` in a Python producer script) acts as a high-throughput message bus for raw clickstream events.
* **Stream Processing:** Apache Spark Structured Streaming (using `PySpark`) consumes data from Kafka, performs real-time transformations, and aggregates metrics (e.g., page views per minute).
* **Serving Layer (Conceptual):** While not fully implemented in this demo for simplicity, in a production environment, aggregated metrics from Spark would be written to a data warehouse like Amazon Redshift for persistent storage or a low-latency cache like Redis for dashboard lookups.
* **Visualization:** A simple, live-updating dashboard built with Streamlit queries the processed data to display the latest metrics.


## 💡 Key Areas

This project will demonstrate:

* Real-time data ingestion using Apache Kafka.
* Distributed stream processing with Apache Spark Structured Streaming.
* Stateful aggregations (like windowing) on streaming data.
* Connecting a data pipeline to a live visualization layer.


## 🛠️ Prerequisites

Before you begin, ensure you have the following installed on your system:

* **Python 3.x:**
    * Check with `python3 --version`.
    * **Recommended (macOS):** Install via Homebrew: `brew install python@3.10` (or newer).
* **Java Development Kit (JDK) 11 or 17:** Spark requires Java.
    * Check with `java -version`.
    * **Recommended (macOS):** Install OpenJDK via Homebrew: `brew install openjdk@11`. Remember to follow Homebrew's instructions to link the JDK and set your `JAVA_HOME` environment variable (e.g., `export JAVA_HOME=$(/usr/libexec/java_home -v 11)` in your `~/.zshrc` or `~/.bash_profile`).
* **Apache Kafka:**
    * Download from <https://kafka.apache.org/downloads>.
    * Extract the archive (e.g., `tar -xzf kafka_2.13-3.x.x.tgz` and move to a convenient location like `~/kafka`).
* **Apache Spark:**
    * Download from <https://spark.apache.org/downloads.html>. Choose a pre-built package for Hadoop 3.3 or later.
    * Extract the archive (e.g., `tar -xzf spark-3.x.x-bin-hadoop3.tgz` and move to a convenient location like `~/spark`).
    * Set `SPARK_HOME` and add Spark's `bin` directory to your `PATH` in your shell profile (e.g., `~/.zshrc` or `~/.bash_profile`):
        ```bash
        export SPARK_HOME=~/spark
        export PATH=$SPARK_HOME/bin:$PATH
        export PYSPARK_PYTHON=python3 # Ensure PySpark uses Python 3
        ```
        Remember to `source` your profile file after making changes.


<br>

### Install Python Libraries 🐍 

In a new terminal, install the required Python libraries:

```bash
pip install kafka-python pyspark streamlit
```

<br>

## ⚙️ Setup Instructions

Follow these steps to get the environment ready:

### 1. Start Apache Kafka 📥

Open **three separate terminal windows** for Kafka operations.

* **Terminal 1: Start ZooKeeper**
    Navigate to your Kafka directory (e.g., `cd ~/kafka`) and run:
    ```bash
    bin/zookeeper-server-start.sh config/zookeeper.properties
    ```
    Keep this terminal open.

  ![image](https://github.com/user-attachments/assets/daf49e40-f52b-41b6-88fe-0c0786bf168b)
  

* **Terminal 2: Start Kafka Broker**
    Open a new terminal, navigate to your Kafka directory, and run:
    ```bash
    bin/kafka-server-start.sh config/server.properties
    ```
    Keep this terminal open.

  ![image](https://github.com/user-attachments/assets/cfd72bfd-c011-4d1f-8333-ada0eb9ec595)
  

* **Terminal 3: Create Kafka Topic**
    Open another new terminal, navigate to your Kafka directory, and create the topic:
    ```bash
    bin/kafka-topics.sh --create --topic user_clickstream --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
    ```
    You should see a confirmation message.

 ![image](https://github.com/user-attachments/assets/f49b0ce1-e1e6-4c36-9d15-a0e477f3e54d)



<br>

### 2. Prepare Spark Kafka Connector JARs 🔗

The PySpark consumer script will automatically download the necessary Kafka connector JARs when you submit the job using the `--packages` argument. You don't need to manually download them beforehand.

<br>


## 📂 Project Files

* `kafka_producer.py`: Python script to simulate and send clickstream events to Kafka.
* `spark_consumer.py`: PySpark Structured Streaming application to consume, process, and aggregate data from Kafka.
* `dashboard_app.py`: Streamlit application to visualize the real-time analytics.

   ![image](https://github.com/user-attachments/assets/109c35bd-66ca-4b81-86a6-b5841125ff5b)

<br>


## ▶️ Running the Project

Open **three separate terminal windows** for running the project components.

<br>


### 1. Run the Kafka Producer 📤

In **Terminal 1**, navigate to the directory where you saved `kafka_producer.py` and run:

```bash
python3 kafka_producer.py
```
This script will start generating and sending simulated user click events to your `user_clickstream` Kafka topic. You'll see "Sent: {...}" messages in the terminal. 

Keep this running.

   ![image](https://github.com/user-attachments/assets/a62df2a2-54e6-4cdd-9299-6f44003382ce)


<br>



### 2. Run the PySpark Structured Streaming Consumer 📈
<br>

In **Terminal 2**, navigate to the directory where you saved `spark_consumer.py` and run the Spark job. Make sure to include the `--packages` argument so Spark can connect to Kafka:

```bash
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.2.0,org.apache.kafka:kafka-clients:2.8.0 spark_consumer.py
```
Spark will start consuming messages from Kafka, processing them, and printing the aggregated results like page views per minute and user engagement to this terminal.

  ![image](https://github.com/user-attachments/assets/752a34bb-03bb-489d-9170-94c04f2ddccf)


<br>


### 3. Run the Streamlit Dashboard 📊

In **Terminal 3**, navigate to the directory where you saved `dashboard_app.py` and run the Streamlit application:

```bash
streamlit run dashboard_app.py
```
This command will open a new tab in your web browser (usually `http://localhost:8501`) displaying the real-time analytics dashboard. The dashboard currently uses simulated data, but in a real setup, it would fetch data from your serving layer (Redshift/Redis).


<br>

   ![image](https://github.com/user-attachments/assets/13d3dbf2-b931-4bba-af59-c863c42cc15f)


<br>


   


<br>


   ![image](https://github.com/user-attachments/assets/a0070811-2a3d-4a53-8c99-aff906ceed4d)



<br>


