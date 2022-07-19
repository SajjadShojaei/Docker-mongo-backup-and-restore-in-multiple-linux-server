# MongoDB Data Backup deployed with Docker

With these commands, we want to store and restore the backup operation of the data deployed on the main server by Docker on another server.

Here we use two methods. The first method is to take a backup on the main server and copy the data to another server, and the second method is to take a backup to another server and transfer the data to a different local directory.

# First method
**Backup**

Take backup of running app data from docker then copy to local folder out of docker.

    docker exec -it containerName mongodump --archive=/root/mongodump.gz --gzip

    docker cp mongodb:/root/mongodump.gz mongodump_$(date +'%m-%d-%y-%H:%M:%S').gz

**Copy backup to server**

Move data to another server/local machine or a backup location

    scp /path/to/dumpfile root@serverip:/path/to/backup

**Restore**

    docker cp /path/to/dumpfile mongodb:/root/mongodump.gz

    docker exec -it mongodb mongorestore --archive=/root/mongodump.gz --gzip


# Second method
**Backup**

Take a backup to another server and transfer the data to a different local directory.

    docker exec containerName sh -c 'mongodump --uri mongodb://user:password@127.0.0.1:27021/databaseName --authenticationDatabase admin  -u user -p password --db databaseName    --archive  --gzip' > /root/mongodump.gz

Here we copy the backup file uniquely with the date and time in the desired folder

    sudo cp /root/mongodump.gz /path/to/backups/$(date +'%m-%d-%y-%H:%M:%S')mongodump.gz

**Restore**

We restore the last backup taken on the second server.

    docker exec -i containerName sh -c 'mongorestore --host 0.0.0.0:27034  --authenticationDatabase admin -u user -p password --db databaseName --archive --gzip' < /root/mongodump.gz


#######################################################################

# Scheduling backup with Cron
You can back up data automatically in a certain period of time by Linux cron commands.

    crontab -e

    */2 * * * * docker exec containerName sh -c 'mongodump --uri mongodb://user:password@127.0.0.1:27021/databaseName --authenticationDatabase admin  -u user -p password --db databaseName    --archive  --gzip' > /root/mongodump.gz
