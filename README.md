# Couchbase Docker Container

Several Couchbase Docker images are available at [Docker Hub](https://hub.docker.com/_/couchbase/). The common ones are listed below:

| Image | Purpose | 
|----------|----------|
| `couchbase` | Latest GA of Couchbase Enterprise Edition |
| `couchbase:community` | Latest GA of Couchbase Community Edition |
| `couchbase/server` |Latest Developer Preview, Beta or GA release of Coucbbase |
| `couchase/server:sandbox` | Latest sandbox GA of couchbase/server |

All images, except `couchbase/server:sandbox`, require Couchbase to be configured before it can be used. The `couchbase/server:sandbox` image is pre-configured developer sandbox to get you started quickly.

## Run Couchbase Container

Open a terminal, start Couchbase Couchbase Docker container using the command:

```
docker run -d --name db -p 8091-8093:8091-8093 -p 11210:11210 couchbase/server:sandbox
```

| warning: This command uses default ports for Couchbase. If you already have a Couchbase instance running on your machine then this command may fail. Make sure to stop the local instance of Couchbase first.

Logs of the container can be seen using the command `docker logs db`.

## Connect using Couchbase CLI

Connect to this Couchbase container using the Couchbase CLI:

```
docker run -it --link db:db couchbase/server:sandbox cbq --engine http://db:8093
```

In this command:

1. `-it` runs the container in an interactive way and allows TTY interaction. This is required for Couchbase CLI to be able to communicate with the container.

1. `--link` creates a symbolic link to the running container. This allows to use the symbolic name to be referred later in the command.

1. `cbq --engine http://db:8093` is the Couchbase CLI that connects using the symbolic name created earlier. This overrides the default command for the container.

It shows the output:

```
No input credentials. In order to connect to a server with authentication, please provide credentials.
 Connected to : http://db:8093/. Type Ctrl-D or \QUIT to exit.

 Path to history file for the shell : /root/.cbq_history
cbq>
```

Run a [N1QL query](http://www.couchbase.com/n1ql) query to get one JSON document from travel-sample bucket:

```
cbq> select * from `travel-sample` limit 1;
{
    "requestID": "aeecafc7-6f9f-4315-8852-8f14d3a07486",
    "signature": {
        "*": "*"
    },
    "results": [
        {
            "travel-sample": {
                "callsign": "MILE-AIR",
                "country": "United States",
                "iata": "Q5",
                "icao": "MLA",
                "id": 10,
                "name": "40-Mile Air",
                "type": "airline"
            }
        }
    ],
    "status": "success",
    "metrics": {
        "elapsedTime": "16.528755ms",
        "executionTime": "16.473524ms",
        "resultCount": 1,
        "resultSize": 300
    }
}
cbq> 
```

Type `Ctrl+D` to quit the `\QUIT` to exit the CBQ shell.

## Run Sync Gateway Container



## Cleanup

Stop the container using the command `docker stop db`.

Remove the container using the command `docker rm db`.

# Summary

Congratulations!

You started a Couchbase Docker container and connected to it using the Couchbase CLI and ran a query.