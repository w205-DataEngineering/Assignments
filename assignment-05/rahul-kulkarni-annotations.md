### copy yml which includes two contiainers - one for redis and one for mids
cp ../course-content/05-Storing-Data-II/example-1-docker-compose.yml docker-compose.yml
### start up the redis cluster and the mids container
docker-compose up -d
### cat the yml file 
cat docker-compose.yml
### validate that the container applications are up and running
docker-compose ps
### check the logs to verify redis is ready to accept client requests
docker-compose logs redis
### exec into the bash shell for the mids container
docker-compose exec mids bash
### from the bash shell execute ipython commands
rahul_kulkarni@myw205tools:~/w205/redis-cluster$ docker-compose exec mids bash
root@b04fb57bad63:~# ipython
Python 3.5.2 (default, Nov 23 2017, 16:37:01) 
Type 'copyright', 'credits' or 'license' for more information
IPython 6.3.0 -- An enhanced Interactive Python. Type '?' for help.
### connect to redis on a specific port, get a handle
In [1]: import redis
In [2]: r = redis.Redis(host='redis',port='6379')
### check what keys are present
In [3]: r.keys()
Out[3]: []
### create a key/value pair 
In [4]: r.set('205','dataengineering')
Out[4]: True
### lookup based on the created key
In [5]: value = r.get('205')
### print the value got for the key
In [6]: print(value)
b'dataengineering'
In [7]: 

### bring down the cluster
docker-compose down
### validate things are cleaned up
docker-compose ps
