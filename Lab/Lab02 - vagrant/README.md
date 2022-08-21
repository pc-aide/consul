# Lab02 - vagrant

---

## github
1. [course2-consul-gs](https://github.com/g0t4/course2-consul-gs)
2. [ddl-course2-consul-gs](https://codeload.github.com/g0t4/course2-consul-gs/zip/refs/heads/master)

---

## Requirements
1. vagrant installed
2. virtualBox installed
3. hypervisor present (bios)
4. no dockerDesktop

---

## Diagram

---

## Steps
### 1 - vagrant up
1. go to the folder .\course2-consul-gs\install\vm
2. vagrant up (Time: ~8min)
[<img src="https://i.imgur.com/m9LNbaA.png">](https://i.imgur.com/m9LNbaA.png)

3. outputs\virtualBox :

[<img src="https://i.imgur.com/ZTi0UVB.png">](https://i.imgur.com/ZTi0UVB.png)

---

### vagrant ssh
1. vagrant ssh
[<img src="https://i.imgur.com/xeWNT95.png">](https://i.imgur.com/xeWNT95.png)

2. vagrant ssh\
````sh
# a --all, executes specified command over all objects
# s --statistics
# 192.168.56.10 = Vagranfile\Ln10
ip -4 a s
````
[<img src="https://i.imgur.com/3FuTtqM.png">](https://i.imgur.com/3FuTtqM.png)

3. vagrant ssh\mount agent dev
````sh
consul agent -dev
````
4. with other machine - not members 
````sh
consul members
````
[<img src="https://i.imgur.com/RaJbuD9.png">](https://i.imgur.com/RaJbuD9.png)

5. add my host to the consul members (vm with consul agent -dev - vm IP)
````sh
# 192.168.56.10 = VM IP
# still error
consul members CONSUL_HTTP_ADDR=http://192.168.56.10:8500 
````
[<img src="https://i.imgur.com/j8emU05.png">](https://i.imgur.com/j8emU05.png)

6. stop consul agent -dev
7. we will bind to 0.0.0.0 = 192.168.56.10
````sh
consul agent -dev -client 0.0.0.0
````
[<img src="https://i.imgur.com/WU5sNvx.png">](https://i.imgur.com/WU5sNvx.png)

8. still refuse ?
````sh
consul members CONSUL_HTTP_ADDR=http://192.168.56.10:8500

Error retrieving members: Get "http://127.0.0.1:8500/v1/agent/mbers?segment=_all": dial tcp 127.0.0.1:8500: connectex: No connection could be made beceause the target machine actively refused it
````
9. test
````sh
# now, it working
consul members -http-addr=http://192.168.56.10:8500
````
[<img src="https://i.imgur.com/ZojqXAu.png">](https://i.imgur.com/ZojqXAu.png)
