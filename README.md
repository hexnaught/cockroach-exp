# CockroachDB Cluster

## With Kubernetes

https://www.cockroachlabs.com/docs/stable/orchestrate-a-local-cluster-with-kubernetes.html

## Local Cluster with Docker Compose

First, start the cluster (optionally detached mode `-d`):

`docker compose up --build`

Then run the `init` command against a node. Below we are running it against `roach1`, the first node in the cluster.

`docker exec -it roach1 ./cockroach init --insecure`

You can then exec in to the contaner with a bash shell, or with the SQL CLI.

`docker exec -it roach1 /bin/bash`
`docker exec -it roach1 ./cockroach sql --insecure`

## Emulating a simple workload

> CockroachDB also comes with a number of built-in workloads for simulating client traffic. Let's the workload based on CockroachDB's sample vehicle-sharing application, MovR.

```sh
# This will initialise the DB for the workload, cockroachdb will automatically propogate the DB through he cluster to other nodes.
docker exec -it roach1 ./cockroach workload init movr 'postgresql://root@roach1:26257?sslmode=disable'

# Run this command in as many shells as you want to run workloads, it can be ran against any node in the cluster. Example below is against roach1 and will run for 5 minutes.
docker exec -it roach1 ./cockroach workload run movr --duration=5m 'postgresql://root@roach1:26257?sslmode=disable'
```
