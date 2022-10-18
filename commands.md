# commands

---

## jq
* jq -format jasonScrip with color between key & value

---

## basic
|n|name|e.g.|O/P|
|-|----|----|---|
|1|version|consul version|[<img src="https://i.imgur.com/nAmjSA7.png">](https://i.imgur.com/nAmjSA7.png)|
|2|help|consul catalog -h|||
|3|info|consul info|[<img src="https://i.imgur.com/bMzmbkz.png">](https://i.imgur.com/bMzmbkz.png)|
|4|autocomplete|# add a line to consul in .bashrc<br/># it'll work with new shell<br/>`consul -autocomplete-install`<br/><br/>`consul -autocomplete-uninstall`|[<img src="https://i.imgur.com/KcSQV0J.png">](https://i.imgur.com/KcSQV0J.png)<br/>tab key: <br/> [<img src="https://i.imgur.com/LJ3orP0.png">](https://i.imgur.com/LJ3orP0.png)|
|5|reload|consul reload||
|6|validate|consul validate \<path\><br/>add: data_dir = "/consul/data" in your *.hcl<br/><br/>`consul validate conf/`|[<img src="https://i.imgur.com/SjYV2AJ.png">](https://i.imgur.com/SjYV2AJ.png)<br/>with data_dir:<br/>[<img src="https://i.imgur.com/V5R9VD7.png">](https://i.imgur.com/V5R9VD7.png)|

---

## agent
|n|name|e.g.|O/P|
|-|----|----|---|
|1|dev |# agent --Runs a Consul agent <br/><br/>-dev --Start the agent in dev mode<br/>`consul agent -dev`|consul agent -dev <br/> ==> Starting Consul agent... <br/> &ensp; Version: '1.12.2' <br/> &ensp; Node ID: 'b00768d0-bf20-4131-e010-9586f58c85cf' <br/> &ensp; Node name: 'vm-terraform' <br/> &ensp; Datacenter: 'dc1' (Segment: '<all>')<br/> &ensp; Server: true (Bootstrap: false) <br/> &ensp; Client Addr: [127.0.0.1] (HTTP: 8500, HTTPS: -1, gRPC: 8502, DNS: 8600) <br/> &ensp; Cluster Addr: 127.0.0.1 (LAN: 8301, WAN: 8302) <br/> &ensp; Encrypt: Gossip: false, TLS-Outgoing: false, TLS-Incoming: false, Auto-Encrypt-TLS: false<br/> [<img src="https://i.imgur.com/VnCtiAQ.png">](https://i.imgur.com/VnCtiAQ.png)<br/>default URL: loopback:8500<br/>[<img src="https://i.imgur.com/NunMgAI.png">](https://i.imgur.com/NunMgAI.png)<br/> url=/v1/internal/ui/services?dc=dc1:<br/>[<img src="https://i.imgur.com/NlkuWob.png">](https://i.imgur.com/NlkuWob.png)<br/>url=v1/catalog/datacenters:<br/> [<img src="https://i.imgur.com/Whz1tj3.png">](https://i.imgur.com/Whz1tj3.png)<br/> url=v1/agent/self<br/>[<img src="https://i.imgur.com/rAefixb.png">](https://i.imgur.com/rAefixb.png)|
|2|stop the agent|consul leave|[<img src="https://i.imgur.com/kb25iCh.png">](https://i.imgur.com/kb25iCh.png)|
|3|addr to bind|# addr 0.0.0.0 = <IP addr host> <br/>`consul agent -dev client 0.0.0.0`||

---
  
## services
|n|name|e.g.|O/P|
|-|----|----|---|
|1|register|`consul services register -name shipments -address 10.0.0.30 -port 8080`<br/><br/># port 8600 - default DNS<br/># checkUp:<br/> `dig "@127.0.0.1" -p 8600 shipments.service.consul`<br/> # output should return IP address 10.0.0.30|[<img src="https://i.imgur.com/kpyx4zP.png">](https://i.imgur.com/kpyx4zP.png)<br/> O/P after dig : <br/> [<img src="https://i.imgur.com/gBYor4A.png">](https://i.imgur.com/gBYor4A.png)|

---
  
## catalog
|n|name|e.g.|O/P|
|-|----|----|---|
|1|services|consul catalog services|[<img src="https://i.imgur.com/k61XjlH.png">](https://i.imgur.com/k61XjlH.png)|
|2|nodes|consul catalog nodes|[<img src="https://i.imgur.com/MpShaJA.png">](https://i.imgur.com/MpShaJA.png)|

---
  
## members
|n|name|e.g.|O/P|
|-|----|----|---|
|1|cluster|consul members|[<img src="https://i.imgur.com/I8Bw7yR.png">](https://i.imgur.com/I8Bw7yR.png)|
|2|-http-addr=\<address\>|consul members -http-addr=http://192.168.56.10.8500|[<img src="https://i.imgur.com/YpCpHR3.png">](https://i.imgur.com/YpCpHR3.png)|

---
  
## watch
|n|name|e.g.|O/P|
|-|----|----|---|
|1|keyprefix|watch value in kv <br/>`consul watch -type keyprefix -prefix /`<br/>`consul watch -type keyprefix -prefix / jq`|[<img src="https://i.imgur.com/4CtJsNh.png">](https://i.imgur.com/4CtJsNh.png)|
|2|services|`consul watch -type services jq`||

---
  
## config
|n|name|e.g.|O/P|
|-|----|----|---|
|1|list|consul config list -ind service-intentions||
|2|write|consul config write conf/centries/deny.orders.shipments.hcl|
