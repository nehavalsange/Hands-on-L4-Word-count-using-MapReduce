## Name: Neha Valsange
## Id: 801393828

## Project Overview

This project is part of Cloud Computing for Data Analysis.
The goal is to implement a Word Count program using Hadoop MapReduce in Java and Maven, then deploy and execute it using Docker.

The program processes a text file and counts the occurrences of each word (with length ≥ 3), outputting the results in descending order of frequency.

## Approach and implementation
1. Mapper

- Reads each line of text.
- Splits the line into words using whitespace and punctuation delimiters.
- Filters out words with length less than 3.
- Emits (word, 1) as the key-value pair.

2. Reducer

- Receives each unique word along with its list of occurrences.
- Sums the counts for each word.
- Outputs (word, total_count).

3. Sorting

The results are sorted in descending order of frequency using Hadoop’s output formatting.

## Setup and Execution

### 1. **Start the Hadoop Cluster**

Run the following command to start the Hadoop cluster:

```bash
docker compose up -d
```

### 2. **Build the Code**

Build the code using Maven:

```bash
mvn clean package
```

### 4. **Copy JAR to Docker Container**

Copy the JAR file to the Hadoop ResourceManager container:

```bash
docker cp target/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 5. **Move Dataset to Docker Container**

Copy the dataset to the Hadoop ResourceManager container:

```bash
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 6. **Connect to Docker Container**

Access the Hadoop ResourceManager container:

```bash
docker exec -it resourcemanager /bin/bash
```

Navigate to the Hadoop directory:

```bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 7. **Set Up HDFS**

Create a folder in HDFS for the input dataset:

```bash
hadoop fs -mkdir -p /input/data
```

Copy the input dataset to the HDFS folder:

```bash
hadoop fs -put ./input.txt /input/data
```

### 8. **Execute the MapReduce Job**

Run your MapReduce job using the following command: Here I got an error saying output already exists so I changed it to output1 instead as destination folder

```bash
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/data/input.txt /output1
```

### 9. **View the Output**

To view the output of your MapReduce job, use:

```bash
hadoop fs -cat /output1/*
```

### 10. **Copy Output from HDFS to Local OS**

To copy the output from HDFS to your local machine:

1. Use the following command to copy from HDFS:
    ```bash
    hdfs dfs -get /output1 /opt/hadoop-3.2.1/share/hadoop/mapreduce/
    ```

2. use Docker to copy from the container to your local machine:
   ```bash
   exit 
   ```
    ```bash
    docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output1/ shared-folder/output/
    ```
3. Commit and push to your repo so that we can able to see your output


## My Input: 
 ```bash
   I built a two-container environment with Docker Compose: one to host
PostgreSQL with seeded data and the other for a Python app that queries and
aggregates it. I was taught how to use Compose for service orchestration and
health checking, as well as forwarding environment variables between containers.
I also got some experience in debugging build contexts and verifying outputs with
mounted volumes. This taught me how containerization makes projects portable
and reproducible.
   ```

## Obtained output: 
 ```bash
and     5
with    3
for     2
taught  2
how     2
environment     2
app     1
outputs 1
health  1
makes   1
This    1
well    1
debugging       1
Compose 1
between 1
data    1
host    1
mounted 1
PostgreSQL      1
checking,       1
forwarding      1
other   1
queries 1
containers.     1
service 1
dataset 1
some    1
containerization        1
Docker  1
two-container   1
it.     1
aggregates      1
projects        1
one     1
variables       1
was     1
built   1
the     1
contexts        1
input   1
verifying       1
build   1
experience      1
use     1
volumes.        1
own     1
portable        1
got     1
Compose:        1
reproducible.   1
that    1
Create  1
your    1
also    1
seeded  1
Python  1
orchestration   1
   ```
