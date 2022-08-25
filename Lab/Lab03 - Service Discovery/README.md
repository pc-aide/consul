# Lab03 - Service Discovery

---

## Requirement
1. [dockerHub-weshigbee/consul2-orders](https://hub.docker.com/r/weshigbee/consul2-orders)
2. [dockerHub-shipments](https://hub.docker.com/r/weshigbee/consul2-shipments)
3. [github-ogham/dog](https://github.com/ogham/dog)
4. [github-course2-consul-gs](https://github.com/g0t4/course2-consul-gs)

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
docker container run --rm -it -p 8500:8500 --name consul-dev consul agent -dev --dns-port 53 -client 0.0.0.0
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

12. orders bash
````ps1
docker container exec -i -t orders bash

# checkUp dns
# O/P: nameserver 172.17.0.4 -- addr. ip consul
# same thing - checkUp dns from container shipments - ns will be 192.168.65.5
cat /etc/resolv.conf
````
[<img src="https://i.imgur.com/CHnWD3c.png">](https://i.imgur.com/CHnWD3c.png)
[<img src="https://i.imgur.com/h3ihWd3.png">](https://i.imgur.com/h3ihWd3.png)

13. checkUp\dig
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

14. dog - cli tool for dns client with color
````ps1
docker container run --rm -i -t weshigbee/consul2-dog

# to test
docker container run --rm -i -t weshigbee/consul2-dog google.com

# dog on docker consul
docker container run --rm -i -t weshigbee/consul2-dog  "@172.17.0.3" consul.service.consul

# dog on docker shipments - nothing
docker container run --rm -i -t weshigbee/consul2-dog  "@172.17.0.3" shipments.service.consul

# dog google.com - query refused
docker container run --rm -i -t weshigbee/consul2-dog  "@172.17.0.3" google.com
````
[<img src="https://i.imgur.com/PH2LJrI.png">](https://i.imgur.com/PH2LJrI.png)
[<img src="https://i.imgur.com/95cq7ii.png">](https://i.imgur.com/95cq7ii.png)
[<img src="https://i.imgur.com/thtf1pV.png">](https://i.imgur.com/thtf1pV.png)
[<img src="https://i.imgur.com/MwA3DdZ.png">](https://i.imgur.com/MwA3DdZ.png)
[<img src="https://i.imgur.com/SgViBTJ.png">](https://i.imgur.com/SgViBTJ.png)

15. register : shipments
````ps1
# consul register : 172.17.0.4 - shipments
# (opt: filter): docker network inpect bridge | jq "[].Containers"
consul services register -name shipments -address 172.17.0.4
````
[<img src="https://i.imgur.com/actwUDU.png">](https://i.imgur.com/actwUDU.png)
[<img src="https://i.imgur.com/mN1VZ8t.png">](https://i.imgur.com/mN1VZ8t.png)

16. checkUp - catalog
````ps1
consul catalog services
````
[<img src="https://i.imgur.com/dtmzvMo.png">](https://i.imgur.com/dtmzvMo.png)

17. checkUp - dns client
````ps1
# checkUp: dog on shipments - reply: yes
docker container run --rm -it weshigbee/consul2-dog "@172.17.0.3" shipments.service.consul
````
[<img src="https://i.imgur.com/ukjvlCP.png">](https://i.imgur.com/ukjvlCP.png)

---

### DNS recursion
1. checkUp - error - normal via /httpbin/get
[<img src="https://i.imgur.com/0c3wdeu.png">](https://i.imgur.com/0c3wdeu.png)
[<img src="https://i.imgur.com/lEkzWTa.png">](https://i.imgur.com/lEkzWTa.png)

2. kill off our consul-dev with flag dns
````ps1
# kill off -dns-port 53 from the consul-dev
# dns 1.1.1.1 - cloudFlare
# refresh dns recursor - it'll work
docker container run --rm -it -p 8500:8500 --name consul-dev consul agent -dev --dns-port 53 -client 0.0.0.0 -recursor 1.1.1.1
````
[<img src="https://i.imgur.com/wYpVX3P.png">](https://i.imgur.com/wYpVX3P.png)

3. checkup - dog consul-dev
````ps1
# checkUp - dog on the consul-dev
docker container run --rm -it weshigbee/consul2-dog "@172.17.0.3" httpbin.org

# checkUp - dog on google
 docker container run --rm -it weshigbee/consul2-dog "@172.17.0.3" google.com 
````
[<img src="https://i.imgur.com/8kxASZB.png">](https://i.imgur.com/8kxASZB.png)
[<img src="https://i.imgur.com/sWQQR9y.png">](https://i.imgur.com/sWQQR9y.png)

4. query
````ps1
# query - dog on the shipments - not response - because we running consul-dev mode
docker container run --rm -it weshigbee/consul2-dog "@172.17.0.3" shipments.service.consul
````

5. stop consul-dev flag
````ps1
git clone https://github.com/g0t4/course2-consul-gs.git
````
6. go to discover\files\compse.yml
7. kill off all containers (consul-dev, orders, & shipments
8. checkUp if any container running ...
````ps1
# ps --process
# -a --all
docker container ps -a
cd .\course2-consul-gs\discover\files\
````
[<img src="https://i.imgur.com/IVh3LWm.png">](https://i.imgur.com/IVh3LWm.png)

9. docker compose up
````ps1
docker compose up
````
[<img src="https://i.imgur.com/eUOBWUv.png">](https://i.imgur.com/eUOBWUv.png)

10. checkUp: orders, shipments & consul-dev in our browser
[<img src="https://i.imgur.com/KFdYeGD.png">](https://i.imgur.com/KFdYeGD.png)
[<img src="https://i.imgur.com/DzG9ApN.png">](https://i.imgur.com/DzG9ApN.png)

11. docker compose restart
````ps1
# restart docker if update discover\files\confg\agnet.hcl - eg. add port dns 53
docker compose restart consul-dev
````

12. docker compose down - destroy
````ps1
docker compose down
````
[<img src="https://i.imgur.com/6Bnfi3E.png">](https://i.imgur.com/6Bnfi3E.png)
[<img src="https://i.imgur.com/Ot9DJCC.png">](https://i.imgur.com/Ot9DJCC.png)

---

### smtp
1. sends email notification

[<img src="https://i.imgur.com/MOTKkVF.png">](https://i.imgur.com/MOTKkVF.png)

2. order submitted, email sent - ubu-docker

[<img src="https://i.imgur.com/2pqUJkn.png">](https://i.imgur.com/2pqUJkn.png)

3. signal up consul-dev if you add smtp file hcl
````ps1
 docker compose kill -s SIGHUP consul-dev
````
[<img src="https://i.imgur.com/qmZuGKG.png">](https://i.imgur.com/qmZuGKG.png)

4. in course2-consul-gs\discover\files\compose.yml\Ln35..Ln42 uncoment if commented
````ps1
# -d --detach
docker compose up mails -d
````

[<img src="https://i.imgur.com/J2rLQjA.png">](https://i.imgur.com/J2rLQjA.png)

5. go to http://127.0.0.1:8025/ - mailHog

[<img src="https://i.imgur.com/TCVWCav.png">](https://i.imgur.com/TCVWCav.png)
[<img src="https://i.imgur.com/AakauBq.png">](https://i.imgur.com/AakauBq.png)

---

### deregister
1. start discover\files\dereg.sh
````ps1
consul services deregister -id shipments
# or, same thing :
consul services deregister conf/shipments.service.hcl

# monitoring - open second terminal
consul monitor

# to get again - shipments
consul reload
````
[<img src="https://i.imgur.com/0GjjRLw.png">](https://i.imgur.com/0GjjRLw.png)
[<img src="https://i.imgur.com/ixzY0ZQ.png">](https://i.imgur.com/ixzY0ZQ.png)

2. register new instance
````ps1
# not exist the instance ship2
# must be another name that shipments - so ship2
consul services register -name shipments -id ship2 -address 172.18.0.50
````
[<img src="https://i.imgur.com/2gGEY8d.png">](https://i.imgur.com/2gGEY8d.png)

3. remove all
````ps1
# -v --detach the volume 
docker compose down -v
````
