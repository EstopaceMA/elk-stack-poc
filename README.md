# ELK STACK POC

## Docker Compose Commands

```
// creates and starts all containers within the docker-compose.yml.
docker compose up
```

```
// stops all containers and removes any networks relating to docker-compose.yml. Leaves volumes that were created intact.
docker compose down
```

```
// same as above, except also removes all volumes that were created.
docker compose down -v
```

```
// if the containers exist but are stopped, this will start all the containers
docker compose start.
```

```
// stops all containers and leaves the environment intact.
docker compose stop
```

## Docker

```
docker system prune -a --volumes
```

We can use a command to copy the ca.crt out of the es01-1 container. Remember, the name of the set of containers is based on the folder from which the docker-compose.yml is running. For example, my directory is “elk-stack-poc” therefore, my command would look like this, based on the screenshot above:

```
docker cp elk-stack-poc-es01-1:/usr/share/elasticsearch/config/certs/ca/ca.crt /tmp/.
```

Once the certificate is downloaded, run a curl command to query the Elasticsearch node:

```
curl --cacert /tmp/ca.crt -u elastic:changeme https://localhost:9200
```

```
{
  "name" : "es01",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "5IEEJPWxQsiuNG_4Oxz3ew",
  "version" : {
    "number" : "8.7.1",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "f229ed3f893a515d590d0f39b05f68913e2d9b53",
    "build_date" : "2023-04-27T04:33:42.127815583Z",
    "build_snapshot" : false,
    "lucene_version" : "9.5.0",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

Success!

Notice that we’re accessing Elasticsearch using localhost:9200. This is thanks to the port, which has been specified via the ports section of docker-compose.yml. This setting maps ports on the container to ports on the host and allows traffic to pass through your machine and into the docker container with that port specified.

---
