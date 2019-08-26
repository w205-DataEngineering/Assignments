# Project 3 Setup

- You're a data scientist at a game development company.  
- Your latest mobile game has two events you're interested in tracking: 
- `buy a sword` & `join guild`...
- Each has metadata

## Project 3 Task
- Your task: instrument your API server to catch and analyze event types.
- This task will be spread out over the last four assignments (9-12).

## Project 3 Task Options 

- All: Game shopping cart data used for homework 
- Advanced option 1: Generate and filter more types of items.
- Advanced option 2: Enhance the API to accept parameters for purchases (sword/item type) and filter
- Advanced option 3: Shopping cart data & track state (e.g., user's inventory) and filter


---

# Assignment 10

## Follow the steps we did in class 


1) A summary type explanation of the example. 
  * For example, for Week 6's activity, a summary would be: "We spun up a cluster with kafka, zookeeper, and the mids container. Then we published and consumed messages with kafka."
2) Your `docker-compose.yml`
3) Source code for the flask application(s) used.
4) Each important step in the process. For each step, include:
 * Bring up the mids, kafka and zk containers
   * docker-compose up -d
   * Create a kafka topic events from the kafka container
   * docker-compose exec kafka kafka-topics --create --topic events --partitions 1 --replication-factor 1 --if-not-exists --   
      zookeeper zookeeper:32181 
      Created topic "events".
    * Add a new event "purchase a fish" to the flask app and validate it from client
    * docker-compose exec mids env FLASK_APP=/w205/assignment-10-rahul-kulkarni/game_api_with_json_events.py flask run --host  
      0.0.0.0
      Serving Flask app "game_api_with_json_events"
      Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
      127.0.0.1 - - [24/Jul/2019 18:28:35] "GET /purchase_a_fish HTTP/1.1" 200 -

    * docker-compose exec mids curl http://localhost:5000/purchase_a_fish
     fish Purchased!


    * Validate from kafka cat that the game api service injected events into the kafka events queue
    docker-compose exec mids kafkacat -C -b kafka:29092 -t events -o
    beginning -e
    {"event_type": "purchase_sword"}
    {"event_type": "purchase_fish"}
    % Reached end of topic events [0] at offset 2: exiting
    
    * Process events from Spark
    
    * docker-compose exec spark pyspark
    * From the pyspark shell read from kafka server all events and load in a local rdd
    * raw_events =       spark.read.format("kafka").option("kafka.bootstrap.servers","kafka:29092").option("subscribe","events").option("startingOffsets", "earliest").option("endingOffsets", "latest").load()
    * build and evnets object form raw events
    * events = raw_events.select(raw_events.value.cast('string')) 
    * Extrac events from the events rdd and print - 
    * import json
    * extracted_events = events.rdd.map(lambda x: json.loads(x.value)).toDF()
    * extracted_events.show()
    * This shows the events which were inejcted in the kakfa bus
    * +--------------+ 
      |    event_type|
      +--------------+
      |purchase_sword|
     | purchase_fish|
    
  


