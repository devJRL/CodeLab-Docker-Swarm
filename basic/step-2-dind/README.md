# Demo of Deploying Container

## Simulate Multi-container

### Prepare DinD(Docker-in-Docker)

```bash
# PREPARE
docker-compose up -d

# CHECK
docker container ls
  # registry Container 1ea
  # manager container 1ea
  # worker container 3ea (01~03)
```

### Ochestration With Swarm

#### Init by `manager` and participate `worker`s using tokens.

```bash
# INIT SWARM TO MANAGER CONTAINER
docker container exec -it manager docker swarm init
  # === RESULT ===
  # Swarm initialized: current node (sqgvxajbrs2b59xo3jl1y887t) is now a manager.
  # To add a worker to this swarm, run the following command:
  #     docker swarm join --token SWMTKN-1-2wv4nrpjdg3tjnpl5mcypv7hp20e88iahefr0bufuslzrs3ir7-aaugswy1uz5o5sruc9he8q11y 172.18.0.3:2377
  # To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

# JOIN SWARM TO WORKER CONTAINER (REPEAT ALL OF THEM)
docker container exec -it worker01 docker swarm join \
--token SWMTKN-1-2wv4nrpjdg3tjnpl5mcypv7hp20e88iahefr0bufuslzrs3ir7-aaugswy1uz5o5sruc9he8q11y manager:2377
  # === RESULT ===
  # This node joined a swarm as a worker.

# CHECK
docker container exec -it manager docker node ls
  # === RESULT ===
  # ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
  # sqgvxajbrs2b59xo3jl1y887t *   6e8f8d461291        Ready               Active              Leader              18.05.0-ce
  # 9w1pdxo9p907x22h3469pyl88     58bdf1954514        Ready               Active                                  18.05.0-ce
  # qvtfo6xuw8o14osqekr7zgz2o     536719e34abf        Ready               Active                                  18.05.0-ce
  # cbzuvsgsp9s99lbs5ewwik7de     cf269e6164dd        Ready               Active                                  18.05.0-ce
```

#### Make `host` to push image to `registry container`.

```bash
# BUILD DOCKER IMAGE USING STEP-1-COMPOSE IMAGE
docker build -t example/echo:latest \
-f "../step-1-compose/Dockerfile-step1" ../step-1-compose/.
  # CHECK
  docker image ls | grep echo

# TAG DOCKER IMAGE FOR REGISTRY CONTAINER
docker image tag example/echo:latest localhost:5000/example/echo:latest
  # CHECK
  docker image ls | grep echo

# PUSH DOCKER IMAGE TO REGISTRY CONTAINER
docker image push localhost:5000/example/echo:latest
  # === RESULT ===
  # The push refers to repository [localhost:5000/example/echo]
  # ...
  # latest: digest: sha256:... size: 2418
```

#### Make `worker`s to pull image from `registry container`.

```bash
# REPEAT WORKERS (3ea)
docker container exec -it worker01 \
docker image pull registry:5000/example/echo:latest
  # === RESULT ===
  # latest: Pulling from example/echo
  # ...
  # Digest: sha256:...
  # Status: Downloaded newer image for registry:5000/example/echo:latest
```

#### Fininsh!

This procedure is the basic method of using private docker registry.

---

## Service

The `service` of docker swarm is the `unit` that containers are `part of application`.

### Make service from registry.

```bash
# RUN 1 SERVICE REPLICA
docker container exec -it manager \
docker service create --replicas 1 \
--publish 8000:8080 \
--name echo_replica \
registry:5000/example/echo:latest
  # === RESULT ===
  # overall progress: 1 out of 1 tasks
  # 1/1: running   [==================================================>]
  # verify: Service converged

# CHECK 1 REPlICA
docker container exec -it manager \
docker service ls

# CHECK 1 CONTAINER
docker container exec -it manager \
docker service ps echo_replica
```

### Scale out the service.

```bash
# SCALE OUT UPTO 6 SERVICE REPLICAS
docker container exec -it manager \
docker service scale echo_replica=6
  # === RESULT ===
  # echo_replica scaled to 6
  # overall progress: 6 out of 6 tasks
  # 1/6: running   [==================================================>]
  # ...
  # 6/6: running   [==================================================>]
  # verify: Service converged

# CHECK 6 REPLICAS
docker container exec -it manager \
docker service ls

# CHECK 6 CONTAINERS
docker container exec -it manager \
docker service ps echo_replica
```
