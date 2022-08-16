# commands

---

## basic
|n|name|e.g.|O/P|
|-|----|----|---|
|1|version|consul version|[<img src="https://i.imgur.com/nAmjSA7.png">](https://i.imgur.com/nAmjSA7.png)|

---

## agent
|n|name|e.g.|O/P|
|-|----|----|---|
|1|dev |# agent --Runs a Consul agent <br/><br/>-dev --Start the agent in dev mode<br/>`consul agent -dev`|consul agent -dev <br/> ==> Starting Consul agent... <br/> &ensp; Version: '1.12.2' <br/> &ensp; Node ID: 'b00768d0-bf20-4131-e010-9586f58c85cf' <br/> &ensp; Node name: 'vm-terraform' <br/> &ensp; Datacenter: 'dc1' (Segment: '<all>')<br/> &ensp; Server: true (Bootstrap: false) <br/> &ensp; Client Addr: [127.0.0.1] (HTTP: 8500, HTTPS: -1, gRPC: 8502, DNS: 8600) <br/> &ensp; Cluster Addr: 127.0.0.1 (LAN: 8301, WAN: 8302) <br/> &ensp; Encrypt: Gossip: false, TLS-Outgoing: false, TLS-Incoming: false, Auto-Encrypt-TLS: false<br/> [<img src="https://i.imgur.com/VnCtiAQ.png">](https://i.imgur.com/VnCtiAQ.png)<br/>default URL: loopback:8500<br/>[<img src="https://i.imgur.com/NunMgAI.png">](https://i.imgur.com/NunMgAI.png)<br/> url=/v1/internal/ui/services?dc=dc1:<br/>[<img src="https://i.imgur.com/NlkuWob.png">](https://i.imgur.com/NlkuWob.png)<br/>url=v1/catalog/datacenters:<br/> [<img src="https://i.imgur.com/Whz1tj3.png">](https://i.imgur.com/Whz1tj3.png)<br/> url=v1/agent/self<br/>[<img src="https://i.imgur.com/rAefixb.png">](https://i.imgur.com/rAefixb.png)|

---
  
# services
|n|name|e.g.|O/P|
|-|----|----|---|
|1|register|`consul services register -name shipments -address 10.0.0.30 -port 8080`<br/><br/># port 8600 - default DNS<br/># checkUp:<br/> `dig "@127.0.0.1" -p 8600 shipments.service.consul`<br/> # output should return IP address 10.0.0.30|[<img src="https://i.imgur.com/kpyx4zP.png">](https://i.imgur.com/kpyx4zP.png)<br/> O/P after dig : <br/> [<img src="https://i.imgur.com/gBYor4A.png">](https://i.imgur.com/gBYor4A.png)|
