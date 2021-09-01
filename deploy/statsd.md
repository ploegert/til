# StatsD

## Connect to aks cluster:

```text
$name= "SOMENAME"
$rg = "$name-core-rg"
$aks = "$name-core-aks"
az aks get-credentials --resource-group $rg --name $aks
```

## Create traffic

then I went and created a new container running debian, and installed netcat \(nc\), and ran a script generating ucp traffic to the endpoint

```bash
# startup a new container & connect interactively
kubectl run -it --rm aks-ssh --image=debian

apt-get update && apt-get install netcat -y

while true; do 
    echo -n "environment.dummy0:$((RANDOM % 100))|c" | nc -w 1 -u 172.16.192.4 18125; 
    echo -n "servicename.dummy1:$((RANDOM % 100))|ms|@0.1" | nc -w 1 -u 172.16.192.4 18125; 
    echo -n "servicename.dummy2:43|c" | nc -w 1 -u 172.16.192.4 18125; 
done
```

```text
172.16.192.4:18125

$pod = "app1-687cd765c5-hqzr9" kubectl exec -it $pod -- echo "deploys.test.myservice:1|c" | nc -w 1 -u 172.16.192.4:18125
```

## startup a new container & connect interactively

```text
$pod = "app1-687cd765c5-hqzr9"
kubectl exec $pod -- apt-get update kubectl exec $pod -- apt-get install -y netcat
apt-get update && apt-get install netcat -y
echo "deploys.test.myservice:1|c" | nc -w 1 -u 172.16.192.4:18125
```



## Send Test Metrics:

```bash
#!/usr/bin/env bash

while true; do echo -n "servicename.statsD_troubleshoot:$((RANDOM % 100))|c" | nc -w 1 -u 127.0.0.1 8125; done

while true; do 
    echo -n "servicename.statsD_troubleshoot:$((RANDOM % 100))|c" | nc -w 1 -u 172.16.192.4 18125; 
    echo -n "servicename.dummy1:$((RANDOM % 100))|ms|@0.1" | nc -w 1 -u 172.16.192.4 18125; 
    echo -n "servicename.dummy2:43|c" | nc -w 1 -u 172.16.192.4 18125; 
done


echo 'test.back.slash:1|c' | nc -C -w1 -u ${STATSD_SERVER} ${STATSD_PORT}
```

### setup StatsD Metrics Server:

```bash
#!/usr/bin/env bash

docker run -d\
 --name graphite\
 --restart=always\
 -p 8080:80\
 -p 2003-2004:2003-2004\
 -p 2023-2024:2023-2024\
 -p 8125:8125/udp\
 -p 8126:8126\
 graphiteapp/graphite-statsd
```

