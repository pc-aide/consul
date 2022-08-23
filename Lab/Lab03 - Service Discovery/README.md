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
docker container run --rm -it -p 3000:3000 --name orders weshigbee/consul2-orders

docker container run --rm -it -p 5000:5000 --name shipments weshigbee/consul2-shipments
````
2. containers & localHost:port

[<img src="https://i.imgur.com/Yjsz8W2.png">](https://i.imgur.com/Yjsz8W2.png)

3. localHost:port\orders - fail querying shipments

[<img src="https://i.imgur.com/PafqLGL.png">](https://i.imgur.com/PafqLGL.png)

4. containers\shipments

[<img src="https://i.imgur.com/fXpAjxn.png">](https://i.imgur.com/fXpAjxn.png)

5. url_shipments

[<img src="https://i.imgur.com/us7qrvl.png">](https://i.imgur.com/us7qrvl.png)

---

### docker's default bridge network
1. checkUp docker's default bridge network
````ps1
# opt: pipeline with jq
# it will see all our network (order: 172.17.0.2/16, shipments : 172.17.0.3/16)
docker network inspect bridge 
````
[<img src="https://i.imgur.com/6i5XFWL.png">](https://i.imgur.com/6i5XFWL.png)

2. bash in orders
````ps1
# -i --interactive
# -t tty
docker container exec -i -t orders bash
````
[<img src="https://i.imgur.com/A8Qa2Fy.png">](https://i.imgur.com/A8Qa2Fy.png)

3. change IPs
````ps1
# from src#
# -c[olor]
# -a[ll]
# -s[tatistics]
ip -c -4 a s
````
[<img src="https://i.imgur.com/TwDwkIt.png">](https://i.imgur.com/TwDwkIt.png)
[<img src="https://i.imgur.com/zjM4FDk.png">](https://i.imgur.com/zjM4FDk.png)
