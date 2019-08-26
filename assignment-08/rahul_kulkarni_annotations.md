### bring up the kafka, mids, spark and hdfs containers
docker-compose up -d
docker-compose logs -f kafka

### Create a topic assessments in kafka
docker-compose exec kafka kafka-topics --create --topic assessments --partitions 1 --replication-factor 1 --if-not-exists --zookeeper zookeeper:32181
 
### exec into the mids container and cat the assessments json to the kafka topic assessments
docker-compose exec mids bash -c \"cat /w205/assessment-attempts-20180128-121051-nested.json | jq '.[]' -c | kafkacat -P -b kafka:29092 -t assessments

### exec into the spark container
docker-compose exec spark pyspark

### read from the kafka topic assessments\n",
raw_assessments = spark.read.format("kafka").option("kafka.bootstrap.servers", "kafka:29092").option("subscribe","assessments").option("startingOffsets", "earliest")
.option("endingOffsets", "latest").load() 


### print the schema
raw_assessments.printSchema()
```
root
 |-- key: binary (nullable = true)
 |-- value: binary (nullable = true)
 |-- topic: string (nullable = true)
 |-- partition: integer (nullable = true)
 |-- offset: long (nullable = true)
 |-- timestamp: timestamp (nullable = true)
 |-- timestampType: integer (nullable = true)
```

### cast as string and write to a tmp file
assessments = raw_assessments.select(raw_assessments.value.cast('string'))

### build a RDD from the assesments json and create a temporary table
extracted_assessments = assessments.rdd.map(lambda x: Row(**json.loads(x.value))).toDF()
extracted_assessments.registerTempTable('assessments')

### select from the table
spark.sql("select keen_id from assessments limit 10").show()
```
+--------------------+
|             keen_id|
+--------------------+
|5a6745820eb8ab000...|
|5a674541ab6b0a000...|
|5a67999d3ed3e3000...|
|5a6799694fc7c7000...|
|5a6791e824fccd000...|
|5a67a0b6852c2a000...|
|5a67b627cc80e6000...|
|5a67ac8cb0a5f4000...|
|5a67a9ba060087000...|
|5a67ac54411aed000...|
+--------------------+
```

    
### get the user incomplete question from the sequences list

spark.sql("select keen_timestamp, sequences.questions[0].user_incomplete from assessments limit 10").show()
```
+------------------+-------------------------------------------------------+
|    keen_timestamp|sequences[questions] AS `questions`[0][user_incomplete]|
+------------------+-------------------------------------------------------+
| 1516717442.735266|                                                   true|
| 1516717377.639827|                                                  false|
| 1516738973.653394|                                                  false|
|1516738921.1137421|                                                  false|
| 1516737000.212122|                                                  false|
| 1516740790.309757|                                                  false|
|1516746279.3801291|                                                  false|
| 1516743820.305464|                                                  false|
|  1516743098.56811|                                                  false|
| 1516743764.813107|                                                  false|
+------------------+-------------------------------------------------------+
```

### extract the sequence id 
spark.sql("select sequences_id from sequences limit 10").show()
```
+--------------------+
|        sequences_id|
+--------------------+
|5b28a462-7a3b-42e...|
|5b28a462-7a3b-42e...|
|b370a3aa-bf9e-4c1...|
|b370a3aa-bf9e-4c1...|
|04a192c1-4f5c-4ac...|
|e7110aed-0d08-4cb...|
|5251db24-2a6e-424...|
|066b5326-e547-4da...|
|8ac691f8-8c1a-403...|
|066b5326-e547-4da...|
+--------------------+  
```

### join sequence id and keen id
spark.sql("select a.keen_id, a.keen_timestamp, s.sequences_id from assessments a join sequences s on a.keen_id = s.keen_id limit 10").show()
```
+--------------------+------------------+--------------------+
|             keen_id|    keen_timestamp|        sequences_id|
+--------------------+------------------+--------------------+
|5a17a67efa1257000...|1511499390.3836269|8ac691f8-8c1a-403...|
|5a26ee9cbf5ce1000...|1512500892.4166169|9bd87823-4508-4e0...|
|5a29dcac74b662000...|1512692908.8423469|e7110aed-0d08-4cb...|
|5a2fdab0eabeda000...|1513085616.2275269|cd800e92-afc3-447...|
|5a30105020e9d4000...|1513099344.8624721|8ac691f8-8c1a-403...|
|5a3a6fc3f0a100000...| 1513779139.354213|e7110aed-0d08-4cb...|
|5a4e17fe08a892000...|1515067390.1336551|9abd5b51-6bd8-11e...|
|5a4f3c69cc6444000...| 1515142249.858722|083844c5-772f-48d...|
|5a51b21bd0480b000...| 1515303451.773272|e7110aed-0d08-4cb...|
|5a575a85329e1a000...| 1515674245.348099|25ca21fe-4dbb-446...|
+--------------------+------------------+--------------------+
```
    