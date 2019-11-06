# Eureka cluster on k8s

## Requirements
1. Java - 1.8.x

2. Maven - 3.x.x  

3. k8s  

4. helm  

5. ingress

## Build jar & docker images & push images

### 1. Build jar
```bash
make java
```
### 2. Build docker image & push image to registry
```bash
make all
```

## Setup in k8s
```bash
helm install --name helm-eureka ./helm-eureka
```

## Stop it in k8s
```bash
helm delete helm-eureka --purge
```

## Test eureka cluster
```bash
curl -X PUT "http://helm-eureka-2.helm-eureka.default.svc.cluster.local:8761/eureka/apps/HELM-EUREKA/helm-eureka:helm-eureka-1.helm-eureka.default.svc.cluster.local:8761/status?value=UP" -uadmin:admin
```
然后查看页面是是否是有一个节点是 `DOWN` 的状态
```bash
curl http://admin:admin@helm-eureka-2.helm-eureka.default.svc.cluster.local:8761
...
<td>
    <b>UP</b> (2) -
        <a href="http://helm-eureka-0.helm-eureka.default.svc.cluster.local:8761/actuator/info" target="_blank">helm-eureka:helm-eureka-0.helm-eureka.default.svc.cluster.local:8761</a>

        <a href="http://helm-eureka-2.helm-eureka.default.svc.cluster.local:8761/actuator/info" target="_blank">helm-eureka:helm-eureka-2.helm-eureka.default.svc.cluster.local:8761</a>

      <font color=red size=+1><b>
    <b>DOWN</b> (1) -
      </b></font>
        <a href="http://helm-eureka-1.helm-eureka.default.svc.cluster.local:8761/actuator/info" target="_blank">helm-eureka:helm-eureka-1.helm-eureka.default.svc.cluster.local:8761</a>

</td>
...
```