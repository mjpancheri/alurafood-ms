# Microservice Java with Spring Boot and RabbitMQ

## Services (with Eureka and Sprint Gateway)
- avaliacao
- pagamentos
- pedidos

## RabbitMQ by Docker

You can configure a cluster with 3 instances as follows, 
after that you can access all in [http://localhost:8085](http://localhost:8085)

### create a network
```
docker network create alura
```

### create the instances
```
docker run -d --rm --net alura --hostname rabbit1 --name rabbit1 -p 8085:15672 -e RABBITMQ_ERLANG_COOKIE=alura_secret rabbitmq:3.10-management
```
```
docker run -d --rm --net alura --hostname rabbit2 --name rabbit2 -p 8086:15672 -e RABBITMQ_ERLANG_COOKIE=alura_secret rabbitmq:3.10-management
```
```
docker run -d --rm --net alura --hostname rabbit3 --name rabbit3 -p 8087:15672 -e RABBITMQ_ERLANG_COOKIE=alura_secret rabbitmq:3.10-management
```

### join instance 2 into the cluster
```
docker exec -it rabbit2 rabbitmqctl stop_app
docker exec -it rabbit2 rabbitmqctl reset
docker exec -it rabbit2 rabbitmqctl join_cluster rabbit@rabbit1
docker exec -it rabbit2 rabbitmqctl start_app
```

### join instance 3 into the cluster
```
docker exec -it rabbit3 rabbitmqctl stop_app
docker exec -it rabbit3 rabbitmqctl reset
docker exec -it rabbit3 rabbitmqctl join_cluster rabbit@rabbit1
docker exec -it rabbit3 rabbitmqctl start_app
```

