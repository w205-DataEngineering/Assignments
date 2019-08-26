# Project 3 Setup

- You're a data scientist at a game development company.  
- Your latest mobile game has two events you're interested in tracking: 
- `buy a sword` & `join guild`...
- Each has metadata

## Project 3 Task
- Your task: instrument your API server to catch and analyze these two
event types.
- This task will be spread out over the last four assignments (9-12).

---

# Assignment 09

## Follow the steps we did in class 
- for both the simple flask app and the more complex one.

### Turn in your `/assignment-09-<user-name>/README.md` file. It should include:
1) A summary type explanation of the example. 
 We spun up a cluster with zookeeper, mids and kafka containers, created a kafka topic events and we started a flask app using the exec command in the mids container. We tested the flask app via curl command to inject test events in the kafka topic. We verified this by reading the events from the kafka topic using kafkacat.
 
2) Your `docker-compose.yml`
3) Source code for the flask application(s) used.
4) Each important step in the process. For each step, include:
   * Bring up the mids, kafka and zk containers
   * docker-compose up -d
   * Create a kafka topic events from the kafka container
   * docker-compose exec kafka kafka-topics --create --topic events --partit
ions 1 --replication-factor 1 --if-not-exists --zookeeper zookeeper:32181
Created topic "events".
   * Run the game api flask application from the mids container
   * docker-compose exec mids env FLASK_APP=/w205/flask-with-kafka-and-spark/game_api_with_json_events.py flask run --host 0.0.0.0
     Serving Flask app "game_api_with_json_events"
     Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
     
   * We can see that game API service is running on local host and on port 5000 and is ready to recieve requests
   * We now will generate events by invoking the game API endpoints, the game api service will be a kafka procucer and injects events in the kafka topic events
   * docker-compose exec mids curl http://localhost:5000/
     This is the default response!
   * This is the server response:  which got a HTTP request and responded with a 200 OK code
   *  127.0.0.1 - - [15/Jul/2019 22:41:17] "GET / HTTP/1.1" 200 -
   * docker-compose exec mids curl http://localhost:5000/purchase_a_sword
      Sword Purchased!
   * Server Response:
   * 127.0.0.1 - - [15/Jul/2019 22:42:28] "GET /purchase_a_sword HTTP/1.1" 200 -
   * Validate from kafka cat that the game api service injected events into the kafka events queue
   * docker-compose exec mids kafkacat -C -b kafka:29092 -t events -o beginning -e
     {"event_type": "default"}
     {"event_type": "purchase_sword"}
     % Reached end of topic events [0] at offset 2: exiting
    
    
  
  
  
  * The output (if there is any).  Be sure to include examples of generated events when available.
  
  * An explanation for what it achieves 
  


