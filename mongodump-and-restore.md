# MongoDB Data Backup deployed with Docker

With these commands, we want to store and restore the backup operation of the data deployed on the main server by Docker on another server.

Here we use two methods. The first method is to take a backup on the main server and copy the data to another server, and the second method is to take a backup to another server and transfer the data to a different local directory.

# First method

Backup

Take backup of running app data from docker then copy to local folder out of docker.

`docker exec containerName sh -c 'mongodump --uri mongodb://user:password@127.0.0.1:27021/databaseName --authenticationDatabase admin  -u user -p password --db databaseName --archive  --gzip' > MongoDumpDB.gz`

``
