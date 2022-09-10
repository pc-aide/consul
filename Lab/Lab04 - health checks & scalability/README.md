# Lab04 - health checks & scalability

---

## Requirement
1. consul
2. jq
3. watch
4. dog

---

## URL
1. [github-github-course2-consul-gs](https://github.com/g0t4/course2-consul-gs)

---

## Diagram

---

## Outputs
### 1) init
1. go to the course2-consul-gs\check\app\compose.yaml
````ps1
# 4 containers - 1) consul, 2) mailhog, 3) consul2-orders, 4) shipments
docker compose up
````
[<img src="https://i.imgur.com/Ldr9foJ.png">](https://i.imgur.com/Ldr9foJ.png)
[<img src="https://i.imgur.com/TidCOul.png">](https://i.imgur.com/TidCOul.png)
[<img src="https://i.imgur.com/nUFEuvi.png">](https://i.imgur.com/nUFEuvi.png)
[<img src="https://i.imgur.com/uLbHfPA.png">](https://i.imgur.com/uLbHfPA.png)

2. run check\app\checks.sh
````zsh
./checks.sh
````
[<img src="https://i.imgur.com/PUmTVfz.png">](https://i.imgur.com/PUmTVfz.png)
