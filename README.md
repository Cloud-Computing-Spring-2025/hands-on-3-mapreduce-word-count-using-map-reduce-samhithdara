
# WordCount-Using-MapReduce-Hadoop

This repository is designed to test MapReduce jobs using a simple word count dataset. According to the assignement we need to complete the map, reduce and controller files of java so that when we use these file in hadoop they will perform those actions and provide us with count of each word in the text we give in the input text file which is database in real world scenerio.

I have added my experience below the each section how i have gone through.

## Objectives

By completing this activity, students will:

1. **Understand Hadoop's Architecture:** Learn how Hadoop's distributed file system (HDFS) and MapReduce framework work together to process large datasets.
2. **Build and Deploy a MapReduce Job:** Gain experience in compiling a Java MapReduce program, deploying it to a Hadoop cluster, and running it using Docker.
3. **Interact with Hadoop Ecosystem:** Practice using Hadoop commands to manage HDFS and execute MapReduce jobs.
4. **Work with Docker Containers:** Understand how to use Docker to run and manage Hadoop components and transfer files between the host and container environments.
5. **Analyze MapReduce Job Outputs:** Learn how to retrieve and interpret the results of a MapReduce job.

Objective is clear as i explained in the top section.

## Setup and Execution

### 1. **Start the Hadoop Cluster**

Run the following command to start the Hadoop cluster:

```bash
docker compose up -d
```
This is command that is used to run docker container by using yaml as config here we are using hadoop env file as well.

### 2. **Build the Code**

Build the code using Maven:

```bash
mvn install
```
As explained maven is used to build package.

### 3. **Move JAR File to Shared Folder**

Move the generated JAR file to a shared folder for easy access:

```bash
mv target/*.jar shared-folder/input/code/
```

### 4. **Copy JAR to Docker Container**

Copy the JAR file to the Hadoop ResourceManager container:

```bash
docker cp shared-folder/input/code/<your-jar-file>.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
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
Above we moved files we need to hadoop as a hop on docker now connecting to docker container.

Navigate to the Hadoop directory:

```bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 7. **Set Up HDFS**

Create a folder in HDFS for the input dataset:

```bash
hadoop fs -mkdir -p /input/dataset
```

Copy the input dataset to the HDFS folder:

```bash
hadoop fs -put ./input.txt /input/dataset
```

### 8. **Execute the MapReduce Job**

Run your MapReduce job using the following command:

```bash
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/<your-jar-file>.jar com.example.controller.Controller /input/dataset/input.txt /output
```
Here we replaced the jar file name with our jar file and executing the controller with input text file and outpur folder to store the output as parameters for controller file.

### 9. **View the Output**

To view the output of your MapReduce job, use:

```bash
hadoop fs -cat /output/*
```

### 10. **Copy Output from HDFS to Local OS**

To copy the output from HDFS to your local machine:

1. Use the following command to copy from HDFS:
    ```bash
    hdfs dfs -get /output /opt/hadoop-3.2.1/share/hadoop/mapreduce/
    ```

2. use Docker to copy from the container to your local machine:
   ```bash
   exit 
   ```
    ```bash
    docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/ shared-folder/output/
    ```
    Copied the output back to our system in same way as docker in between.
3. Commit and push to your repo so that we can able to see your output