# Real-Time-Streaming-Dashboard-Kafka-Streamlit-SQLite-Python

### Prerequesites: 



#### Install Java

#### I ran `java -version`  to check if Java was installed

<br>

![image](https://github.com/user-attachments/assets/13563f0f-53c1-48e4-8330-fdf774475201)

<br>



#### Downloaded Kafka (binary) scala 2.13 from the Apache Kafka website

![image](https://github.com/user-attachments/assets/b8606283-237d-4589-bbf9-d07b07e73e3a)

<br>




#### Extracted the downloaded file into a custom folder on my local machine 


![image](https://github.com/user-attachments/assets/f4371b3a-2f5a-4a04-958b-fd18165b5aeb)


<br>


### Run Zookeeper and Kafka via commands


#### Step 1 - Start Zookeeper: 

##### Left this command running and created a new terminal window. 

`cd C:\kafka
.\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
`

![image](https://github.com/user-attachments/assets/c27047c5-b5f8-48e7-9c78-736bbf518059)


<br>

![image](https://github.com/user-attachments/assets/1859dabc-7924-416d-be6b-90e5440c75ec)

<br>



#### Step 2 - Open a new PowerShell window for Kafka broker: 

##### Left this command running and created a new terminal window. 

`cd C:\kafka
.\bin\windows\kafka-server-start.bat .\config\server.properties
`


![image](https://github.com/user-attachments/assets/23f538ff-d2ac-4ff2-b56c-3d9f57841fa1)

<br>




#### Step 3 - Open a third PowerShell window to create Kafka topic: 


`cd C:\kafka
.\bin\windows\kafka-topics.bat --create --topic clickstream --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
` 

<br>

You should see a success message like:

Created topic clickstream.

![image](https://github.com/user-attachments/assets/066cdf12-ed07-4ce8-a55c-06a6558aa891)


<br> 


#### Step 4 - Setup Python environment and install dependencies


`python -m venv realtime-env
.\realtime-env\Scripts\Activate.ps1
pip install kafka-python streamlit pandas
` 

![image](https://github.com/user-attachments/assets/c59862f1-4999-4651-8f87-460934b1bf21)


<br>

#### Overview of terminals all four terminal's made:

![image](https://github.com/user-attachments/assets/fcd483e2-0521-4e63-8e92-51364a3bbdee)


<br> 


#### Step 5 - Create Python files in one folder (e.g., C:\real_time_dashboard):


- kafka_producer.py

- kafka_consumer.py

- database.py

- dashboard.py


