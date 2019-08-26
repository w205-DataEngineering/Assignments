# Project 2: Tracking User Activity

- In this project, you work at an ed tech firm. You've created a service that delivers assessments, and now lots of different customers (e.g., Pearson) want to publish their assessments on it. You need to get ready for data scientists who work for these customers to run queries on the data. 

- Through 3 different activites, you will spin up existing containers and prepare the infrastructure to land the data in the form and structure it needs to be to be queried. 
  1) Publish and consume messages with kafka.
  2) Use spark to transform the messages.
  3) Use spark to transform the messages so that you can land them in hdfs.

_______________________________________________________________________________________________________

## Assignment 08

- In this assignment, you'll spin up a cluster with kafka, zookeeper, spark and hdfs containers as well as the mids container. We're adding transforming the messages so that you can land them in hdfs with spark to last week's activity.


### Follow the steps we did in class for the github data with the assessment data with a few changes:

- come up with an appropriate name for your topic
- use the right dataset name in your docker-compose exec command
- When you get to the "Extract Data" step, choose which assessment data columns you want to promote to be stand alone dataframe columns based on the Project 2  description. Explain your choice. 


#### What you turn in:
- In your `/assignment-08-<user-name>` repo:
	* your `docker-compose.yml` 
	* once you've run the example on your terminal
	  * Run `history > <user-name>-history.txt`
	  * Save the relevant portion of your history as `<user-name>-annotations.md`
	  * Annotate the file with explanations of what you were doing at each point.

