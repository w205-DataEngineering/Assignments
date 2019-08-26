# My annotations, Assignment 6


### Fetch the JSON using Curl 
curl -L -o assessment-attempts-20180128-121051-nested.json https://goo.gl/ME6hjp

### bring up the mids, kafka and zookeeper containers
docker-compose up -d

### look at log output for the kafka container
docker-compose logs -f kafka

### Exec into the kafka container and create a topic "asgn6"  using the kafka-topics command and parameters
docker-compose exec kafka kafka-topics --create --topic asgn6 --partitions 1 --replication-factor 1 --if-not-exists --zookeeper zookeeper:32181

### Verify that the topic is created
docker-compose exec kafka kafka-topics --describe --topic asgn6 --zookeeper zookeeper:32181

### Exec into the MIDS container and cat the JSON file, run some JQ commands to check the JSON data
docker-compose exec mids bash -c "cat /w205/kafka/assessment-attempts-20180128-121051-nested.json"
docker-compose exec mids bash -c "cat /w205/kafka/assessment-attempts-20180128-121051-nested.json | jq '.'"
docker-compose exec mids bash -c "cat /w205/kafka/assessment-attempts-20180128-121051-nested.json | jq '.[]' -c"

### Exec into the mids container and cat the output of the JSON file and pipe into using the kafkacat tool in producer mode to the kafka cluster and topic asgn-6 which we just created 
docker-compose exec mids bash -c "cat /w205/kafka/assessment-attempts-20180128-121051-nested.json | jq '.[]' -c | kafkacat -P -b kafka:29092 -t asgn-6 && echo 'Produce messages on kafka topic asgn-6'"

### verify that the produced messages can be consumed from the kafka container using the kafa-console-consumer command
docker-compose exec kafka kafka-console-consumer --bootstrap-server kafka:29092 --topic asgn-6 --from-beginning --max-messages 50

### Exec into the mids container and consume the messages via kafkacat
docker-compose exec mids bash -c "kafkacat -C -b kafka:29092 -t asgn-6 -o beginning -e" 

### Get a count of the messages consumed 
docker-compose exec mids bash -c "kafkacat -C -b kafka:29092 -t asgn-6 -o beginning -e"  | wc -l
