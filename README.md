## 1. Install kafka

`tar -xzf kafka_2.12-2.4.1.tgz`

`cd kafka_2.12-2.4.1`

`bin/zookeeper-server-start.sh config/zookeeper.properties`

`bin/kafka-server-start.sh config/server.properties`

## 2. Create a topic

`bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic input_topic`

`bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic output_topic`

## 3. Use the kafka producer from kafka itself to send test data to my topic

Download the sample raw data from here: https://kafkarawdata.s3.us-east-2.amazonaws.com/stream.jsonl.gz

`gzcat stream.jsonl.gz |../../kafka_2.12-2.4.1/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic input_topic`

## 4. Create a small app that reads this data from kafka and prints it to stdout

See STEP 4 in the main.ipynd

## 5. Find a suitable data structure for counting and implement a simple counting mechanism, output the results to stdout

HashTable/Dictionary: comprehensive, efficient way to count unique users

advanced solution

- Bitmap

See STEP 5 in the main.ipynd

## 6. Benchmark

See STEP 6 in the main.ipynd

Use `%timeit` to check the perfermance of two different output methods

## 7. Output to a new Kafka Topic instead of stdout

See STEP 7 in the main.ipynd

## 8. Try to measure performance and optimize
We can measure the memory footprint, efficiency, accuracy of the algorithm by generating sample input datasets of differnet configuration.

sample input dataset:

	- short time duration
	- large number of records
	- small number of unique users
	- or a combination of the above metrics

## 9. Scale consideration

It can be scaled using parallelization. Using MapReduce to achieve multi-threading process. 

First partition the topic. Then count the unique ID per minite in each part. Next create a Hashset to aggregate the unique ID in the same minute. Finally print out the minite and its hashset length as the unique ID count per minite.

We can also try different algorithm. Using bitmap is one possible way, which is very memory efficient. Bitmap is a great data structure to consider whenever memory space is at a premium: OS kernels (memory pages, inodes etc), digital imaging etc. However in this way we probably will have to sacrifice the result accuracy due to collision.

## 10. Edge cases, options and other consideration

We can improve the algothim's accuracy by continuing counting for the previous minute within certain time or record limit. This is helpful especially when the input data is not exactly in order. For example, we could set a latency scheme to print out: we continue to count for the previous minite for another 5s. or for another 1000 records, before we print out the count of unique IDs at previous minute. 

