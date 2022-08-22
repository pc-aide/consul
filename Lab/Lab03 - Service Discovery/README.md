# Lab03 - Service Discovery

---

## Requirement
1. [dockerHub-weshigbee/consul2-orders](https://hub.docker.com/r/weshigbee/consul2-orders)
2. [dockerHub-shipments](https://hub.docker.com/r/weshigbee/consul2-shipments)

---

## Diagram

---

## Outputs
1. docker cli
````sh
# -rm --remove container to the end
# -i --interactive
# -t tty
# -p --port hostPort:containerPort
docker container run -rm -it -p 3000:3000 --name orders weshigbee/consul2-orders
````
