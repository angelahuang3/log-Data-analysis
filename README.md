# Top K frequent data + log data analytics

## PA1: Word Count (**Python**)
In this assignment, we are going to find the top k frequent word in the big dataset, which is 300MB, 2.5GB and 16GB. We present an analysis of the performance of our implementation of a top-K most frequent words algorithm using different input dataset sizes and optimization techniques. We evaluate the algorithm's efficiency in terms of execution time, memory usage, and CPU utilization, and we analyze the impact of different algorithms, data structures, and system resources on its performance. 

**How to use it**
1. Run "topkwords.py"

2. input the filepath and stopword path

3. choose how many words you would like to see (k)

4. set the sharding number

5. set the num_processes(default: 4)


## PA2: Word Count (**Python + Hadoop MapReduce**)
Finding top 100 words using mapreduce

**How to run it**
1. some simple command line for hdfs
```bash
hadoop fs -mkdir [-p] <hadoop_location>
hadoop fs -put <your_localfile_location> <hadoop_location>
hadoop fs -rm -r <hadoop_location>
```
2. first, run it locally to check
```bash
cat <your_datafile_path> | <mapper.py path> | sort -k1,1 | <reducer.py path>
```
3. run in hadoop
```bash
hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.3.5.jar \
-file <mapper.py path> -mapper <mapper.py path> \
-file <reducer.py path> -reducer <reducer.py path> \
-file <combiner.py path> -combiner <combimer.py path>
-file <stopword.txt path> -input <hadoop_data_path> -output <hadoop_output>
```
4. check the result
```bash
 hadoop dfs -cat <hadoop_output>/part-00000   
```

## PA3: Word Count (**Apache Spark**) + NASA log data analytics(**PySpark + Kafka + DBFS(Databricks) + HDFS**)

**Part 1**
**Determine 100 most frequent/repeated words in the 16GB dataset**

**How to run it**
1. run at local
```python
python3 wc_spark.py
```

2. run at spark
```python
spark-submit --master local wc_spark.py
```

**Part 2**
**Section 1**
**Solving and getting familiar with log analytics.**

1. Run section1.py file to get the freq day of the week and least year for the data

**Section 2**
**Basic**
1. run Kafka and ZooKeeper
*make sure to get into Kafka file location*

```bash
# Start the ZooKeeper service
bin/zookeeper-server-start.sh config/zookeeper.properties

# Start the Kafka broker service
bin/kafka-server-start.sh config/server.properties

```
2. create a topic to store your event

```bash
bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
```
3. show you details such as the partition count of the new topic

```bash
bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
```
4. write some events into topic

```bash
bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
```
5. read the events

```bash
bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
```
**How to run**
1. Run Kafka & Zookeeper
2. Create topic
3. Run kafka_producer.py to input the data into topic
4. Run kafka_consumer.py to save the analysis result into hdfs

