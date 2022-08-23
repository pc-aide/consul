# Lab03 - Service Discovery

---

## Requirement
1. [dockerHub-weshigbee/consul2-orders](https://hub.docker.com/r/weshigbee/consul2-orders)
2. [dockerHub-shipments](https://hub.docker.com/r/weshigbee/consul2-shipments)
3. [github-ogham/dog](https://github.com/ogham/dog)

---

## Diagram
[<img src="https://i.imgur.com/VU8gh2a.png">](https://i.imgur.com/VU8gh2a.png)

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

3. own IPs
````ps1
# from src#
# -c[olor]
# -a[ll]
# -s[tatistics]
ip -c -4 a s
````
[<img src="https://i.imgur.com/TwDwkIt.png">](https://i.imgur.com/TwDwkIt.png)
[<img src="https://i.imgur.com/zjM4FDk.png">](https://i.imgur.com/zjM4FDk.png)

4. listening
````sh
# -l --listening
# from src#
netstat -l
````
[<img src="https://i.imgur.com/VmHcVeT.png">](https://i.imgur.com/VmHcVeT.png)
[<img src="https://i.imgur.com/oLIBpHC.png">](https://i.imgur.com/oLIBpHC.png)

5. env :
````ps1
# -e --environment
# 172.17.0:5000 : shipments
docker container run --rm -it -p 3000:3000 --name orders -e "SHIPMENTS_URL=http://172.17.0.3:5000" weshigbee/consul2-orders
````
[<img src="https://i.imgur.com/Ggm6mGE.png">](https://i.imgur.com/Ggm6mGE.png)
[<img src="https://i.imgur.com/s046pPk.png">](https://i.imgur.com/s046pPk.png)
[<img src="https://i.imgur.com/op679XP.png">](https://i.imgur.com/op679XP.png)

6. consul with flag dns
````ps1
# default port dns: 8600
docker container run --rm -it -p 8500:8500 --name consul-dev consul agent -dev -dnsp-port 53 -client 0.0.0.0
````
[<img src="https://i.imgur.com/0ZaGiTZ.png">](https://i.imgur.com/0ZaGiTZ.png)

7. get the sh in the condul-dev
````ps1
docker container exec -i -t consul-dev sh
````
[<img src="https://i.imgur.com/iF33B1o.png">](https://i.imgur.com/iF33B1o.png)

8. get the ip
````ps1
# O/P: 172.17.0.4/16
ip -4 a s
````
[<img src="https://i.imgur.com/nRykLu5.png">](https://i.imgur.com/nRykLu5.png)

9. checkUp listening
````sh
# -l --listener
netstat -l
````
[<img src="https://i.imgur.com/T8NVTOZ.png">](https://i.imgur.com/T8NVTOZ.png)

10. checkUp don't resolve name
````sh
# -n --don't resolve names
netstat -ln
````
[<img src="https://i.imgur.com/4IM7NPf.png">](https://i.imgur.com/4IM7NPf.png)

11. dns - consul with shipments & orders (all connection together)
````ps1
# --dns -the IP addr. of dns server: consul : 172.17.0.4
# in msEdge orders\report\ error - can't resolve IP add : shipments.service.consul
docker container run --rm -it -p 3000:3000 --name orders -e "SHIPMENTS_URL=http://shipments.service.consul:5000" --dns 172.17.0.4 weshigbee/consul2-orders
````
[<img src="https://i.imgur.com/QrtT8Vc.png">](https://i.imgur.com/QrtT8Vc.png)

11. orders bash
````ps1
docker container exec -i -t orders bash

# checkUp dns
# O/P: nameserver 172.17.0.4 -- addr. ip consul
# same thing - checkUp dns from container shipments - ns will be 192.168.65.5
cat /etc/resolv.conf
````
[<img src="https://i.imgur.com/CHnWD3c.png">](https://i.imgur.com/CHnWD3c.png)
[<img src="https://i.imgur.com/h3ihWd3.png">](https://i.imgur.com/h3ihWd3.png)

12. checkUp\dig
````ps1
# from orders bash
docker container exec -i -t orders bash

# SRC 
# no answer
dig shipments.service.consul
dig consul.service.consul # yes answer: 1 (one record)
````
[<img src="https://i.imgur.com/CdTw7gc.png">](https://i.imgur.com/CdTw7gc.png)
[<img src="https://i.imgur.com/oqA1PHV.png">](https://i.imgur.com/oqA1PHV.png)
