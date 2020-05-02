# Probes


Probes are similar to health checks performed by LoadBalancers. Kubernetes assumes that, as long as container is running, the status of the corresponding pod is also healthy and it shows the pod in running status. However, that is not always the case. There might be a bunch of process still pending inside the container before the pod is truly operational. And even once the pod is operational, what should happen if the pod moves to an undesired status like "Failed" or "CrashloopbackOff" ?

This essentially leads to two variatons of checks - 

* When should kubernetes services qualify a pod as running and start sending traffic to the pod 
* What should happen in case a Pod goes into failure status

### Probes in kubernetes 

In order to cope with the 2 scenarios discussed above, kubernetes provides us with 2 different types of probes - 

* Readiness Probe
  * Used it check if pod is ready to serve traffic 
  * Can be used to check startups and external dependeicies 
  
* Liveness Probe 
  * Detects when a pod enters an undesired state
  * Tells kubernetes when to actually restart a pod 
  
## How to use Probes ?

* Probes are declared in the pod definition 
* For multi-container pod with probes for each container, all probes must pass for the pod to be functional 
* Probe actions provides a way to perform HTTP/GET command and opening TCP socket  

## Demo 

* Create a namespace 

```
kubectl create ns probedemo
```

* Deploy an app container 

```
kubectl create -f redis.yaml -n probedemo
```

Verify the Liveness and Readiness probe defined in the pod - 

```
        livenessProbe:
          tcpSocket:
            port: redis # named port
          initialDelaySeconds: 15
        readinessProbe:
          exec:
            command:
            - redis-cli
            - ping
          initialDelaySeconds: 5

```
