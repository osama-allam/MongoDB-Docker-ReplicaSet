# MongoDB-Docker-ReplicaSet
How to create local docker replica set:
	1. Open CMD as admin
	2. Pull official MongoDB image 
		a. Run --> docker pull mongo
	3. Create docker network
		a. Run --> docker network ls "check existing networks"
		b. Run --> docker network create mongodb-cluster
	4. Setting up our containers
		a. To start up our first container, "mongo1" run the command:
			i. Run --> docker run -d --net mongodb-cluster -p 27017:27017 --name mongo1 mongo mongod --replSet docker-replica --port 27017
			ii. Run --> docker run -d --net mongodb-cluster -p 27018:27018 --name mongo2 mongo mongod --replSet docker-replica --port 27018
			iii. Run --> docker run -d --net mongodb-cluster -p 27019:27019 --name mongo3 mongo mongod --replSet docker-replica --port 27019
	5. Setting up replication
		a. Run --> docker exec -it mongo1 mongosh
		b. Run --> config = {"_id" : "docker-replica","members" : [{"_id" : 0,"host" : "mongo1:27017"},{"_id" : 1,"host" : "mongo2:27018"},{"_id" : 2,"host" : "mongo3:27019"}]}
	6. Setting priorities for each node
		a. Run --> rs.initiate(config)
		b. Run --> cfg = rs.conf()
		c. Run --> cfg.members[0].priority = 1
		d. Run --> cfg.members[1].priority = 0.5
		e. Run --> cfg.members[2].priority = 0.5
		f. Run --> rs.reconfig(cfg)
		g. Run --> rs.status() //to check status for each replica set
	7. Update windows hosts file
		a. Add --> 127.0.0.1 mongo1 mongo2  mongo3 (optional step you can still use localhost in connections string as below)
	8. Ensure all nodes are running from containers tab
	9. Connection string will be "mongodb://localhost:27017/tpay-nosql-db?readPreference=primary&appname=MongoDB%20Compass&directConnection=true&ssl=false"
	10. Create DB with name "tpay-nosql-db" then create collection with name "subscriptions"
![image](https://user-images.githubusercontent.com/14058038/189492479-4a045f89-dd58-435b-81a2-e6ceb9efeade.png)
