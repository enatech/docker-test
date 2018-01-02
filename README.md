# The main files in this folder are to start
# replicas of a web app running counter
# and incrementing it.

# The msatest folder has the same files but the 
# Dockerfile is for running the same app on 8080
# to test if two set of services can access the same DB.

Steps-
Build the image
1.) docker build -t helloworld .
2.) docker images
 
Init the swarm and start containers in it 
3.) docker swarm init
4.) docker stack deploy -c docker-compose.yml helloworldservice
5.) docker service ps helloworldservice
6.) docker service ls
7.) docker service ps helloworldservice_web
8.) docker stack rm helloworldservice
9.) docker swarm leave --force

Now for the secord service, it errors out as the DB is already running on 6379 port.
So remove the DB from first helloworld application docker compose file, run it
as a separate service and then connect via host ip of that container and the port.

10.) edit the docker-compose.yml and remove redis portion
11.) run the redis -
docker run -d --name redis -p 6379:6379 redis

Ideally first run helloworld single instance and let it connect to redis
docker run -d --name helloworldapp -p 80:80 --link redis:redis helloworld

Now start the other helloworlddup single instance and let it connect to same redis
docker run -d --name helloworlddupapp -p 8080:8080 --link redis:redis helloworlddup

Now run in swarm mode
12.) links are not supported in docker swarm mode so configure overlay network
