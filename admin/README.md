Administrator guide
===================

## Monitoring health

A lot of common issues such as not enough disk space, that some service
is down can be detected by a monitoring software, it's simple.

There can be used a `infracheck` container which would expose a JSON
endpoint for an external monitoring service. External monitoring service
can check this endpoint, if a failure is detected, then send a
notification to Slack/Mattermost or other instant messenger.

**Example of an endpoint for externl monitoring:**

```json
{
    "checks": {
        "disk-space": {
            "hooks_output": "",
            "ident": "disk-space=True",
            "output": "There is 520.1GB disk space at '/', nothing to worry about, defined minimum is 6GB\n",
            "status": true
        },
        "docker-health": {
            "hooks_output": "",
            "ident": "docker-health=True",
            "output": "Docker daemon reports that there is no 'unhealthy' service running in '' space\n",
            "status": true
        },
        "minio": {
            "hooks_output": "",
            "ident": "minio=True",
            "output": "",
            "status": true
        },
        "replication-running": {
            "hooks_output": "",
            "ident": "replication-running=True",
            "output": "Replica seems to be in a good state\n",
            "status": true
        },
        "storage-synchronization": {
            "hooks_output": "",
            "ident": "storage-synchronization=True",
            "output": "Storage synchronization looks fine\n",
            "status": true
        }
    },
    "global_status": true
}
```

You should have a link to this health check endpoint, or the link should
be in a notification content sent by a monitoring software. From it you
can read what potentially could be not working.

## Example scenarios

##### 1. Some service is down

```bash

# list all services that names begins with "backups"
# to see which are "exited"
docker ps -a |grep backups

# replace "backups_app_storage_1" with a container name returned by docker ps
# to see logs to better understanding of the issue
docker logs -f backups_app_storage_1

# try to restart the service, after that check the state again with docker ps
docker restart backups_app_storage_1
```

##### 2. There is no disk space

If the services are not working and it's urgent, then you may want to
delete old logs, or all logs from the system, just to restore everything
back online.

```bash

# go to the logs directory and delete old files with ".1.gz" ".2.gz" etc. in name
# check also systemd logs, they can be very large
cd /var/log
```

Docker also can keep too much data in the /var/lib/docker, there are
ready to use commands to clean up what is needed.

```bash
docker image prune
docker container prune
```

After that you can bring back the services.

```bash
cd /project

make start
[ctrl + c]
```

##### 3. Redis does not want to get up

If the REDIS server is used only for caching purposes, not for storing
important data - then you can just delete the REDIS data if it is
corrupted after unexpectedly killed process.

