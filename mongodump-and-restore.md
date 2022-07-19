# MongoDB Data Backup deployed with Docker

With these commands, we want to store and restore the backup operation of the data deployed on the main server by Docker on another server.

Here we use two methods. The first method is to take a backup on the main server and copy the data to another server, and the second method is to take a backup to another server and transfer the data to a different local directory.

# Second method
**Backup**

Take a backup to another server and transfer the data to a different local directory.

`docker exec containerName sh -c 'mongodump --uri mongodb://user:password@127.0.0.1:27021/databaseName --authenticationDatabase admin  -u user -p password --db databaseName --archive  --gzip' > mongodump.gz`

`sudo cp /root/mongodump.gz /path/to/backups/$(date +'%m-%d-%y-%H:%M:%S')mongodump.gz`


