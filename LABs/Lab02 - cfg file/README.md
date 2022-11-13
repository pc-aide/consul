# Lab03 - config

---

## Diagram

---

## Requirements
1. net-tools
2. consul

---

## Ports
1. 8500 - consul web

---

## flags
1. server --Providing this flag specifieds that you want the agent to start in server mode
2. -data-dir -- Storing state data

---

## Consul-SRV
````sh
# bootstrap-expect --How many servers the datacenter should have in total
# -node=Overwride Name. By default, Consul use the hostname of the host
# -bind --Address agent will listen
# -client --Sets the address to bind for client access (RPC,DNS, HTTP/S & gRPC)
# -client=0.0.0.0 --everyone
# ui --user interface
# & --background process
consul agent -server -bootstrap-expect=1 -node=consul-srv -bind=142.93.145.48 -client=0.0.0.0 -data-dir=/root/consul -ui=True &

# checkUp members
consul members

# cfg file
# create the folder if not exist
vim /root/consul/consul-server-config/consul.hcl

data_dir = "/root/consul"
bind_addr = "142.93.145.48"
client_addr = "0.0.0.0"
bootstrap_expect = 1
node_name = "Consul-SRV"
ui = true
server = true

# config-dir --Configuration std location is /etc/consul.d
consul agent -config-dir=/root/consul/consul-server-config/ &
````

---

## Consul-Client
````sh
# cfg folder
mkdir -p /root/consul/consul-client-config

# consul.hcl
vim /root/consul/consul-client-config/consul.hcl

data_dir = "/root/consul"
start_join = ["142.93.145.48"]
bind_addr = "68.183.200.215"
node_name = "Consul-Client"

# start consul client-join to the srv
consul agent -config-dir=/root/consul/consul-client-config/ &
````
