{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Bring up spark, kafka and mids containers\n",
    "docker-compose up -d\n",
    "docker-compose logs -f kafka\n",
    "\n",
    "### Create a kafka topic mids-205-7\n",
    "docker-compose exec kafka kafka-topics --create --topic mids-205-7 --partitions 1 --replication-factor 1 --if-not-exists --zookeeper zookeeper:32181\n",
    "\n",
    "### Describe topic\n",
    "docker-compose exec kafka kafka-topics --describe --topic mids-205-7 --zookeeper zookeeper:32181\n",
    "\n",
    "### List the curled JSON\n",
    "ls -l ~/w205/assessment-attempts-20180128-121051-nested.json \n",
    "\n",
    "### cat the JSON into the kafka topic using kafka cat\n",
    "docker-compose exec mids bash -c \"cat /w205/assessment-attempts-20180128-121051-nested.json | jq '.[]' -c | kafkacat -P -b kafka:29092  -t mids-205-7 && echo 'Produced messages.'\"\n",
    "\n",
    "### exec into pyspark shell\n",
    "docker-compose exec spark pyspark\n",
    "\n",
    "### read  into spark from the kafka topic mids-205-7 and consume messages\n",
    "messages = spark.read.format(\"kafka\").option(\"kafka.bootstrap.servers\", \"kafka:29092\").option(\"subscribe\",\"mids-205-7\").option(\"startingOffsets\", \"earliest\").option(\"endingOffsets\", \"latest\").load() \n",
    "\n",
    "### check the messages\n",
    "messages.show()\n",
    "\n",
    "### convert them to legit strings for human readability\n",
    "messages_as_strings=messages.selectExpr(\"CAST(key AS STRING)\", \"CAST(value AS STRING)\")\n",
    "\n",
    "messages_as_strings.select('value').take(1)\n",
    "### get the first message\n",
    "messages_as_strings.select('value').take(1)[0].value\n",
    "\n",
    "### import JSON and load the first message to unroll\n",
    "import json\n",
    "first_message=json.loads(messages_as_strings.select('value').take(1)[0].value)\n",
    "\n",
    "### check one of the questions by nesting into sequences - questions - user-incomplete property\n",
    "print(first_message['sequences']['questions'][0]['user_incomplete'])\n",
    "True\n"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.7"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
