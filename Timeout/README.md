1)Deploy the destination rule and virtual services
---------------------------------------------------

* Create DestinationRule
```
istioctl create -f destination-rule-recom-v1-v2.yml -n tutorial
```

* Remplace VirtualService to repart the traffic 50% 50%
```
istioctl replace -f virtual-services-recommendation-v1-v2-50-50.yml  -n tutorial
```

2) Install Siege and check the service
--------------------------------------
In this case we have an average time for both Services v1 & v2

```
oc project tutorial

oc get route

siege -r 2 -c 20 -v http://customer-tutorial.app.192.168.33.2.xip.io

** SIEGE 3.1.4
** Preparing 20 concurrent users for battle.
The server is now under siege...
HTTP/1.1 200   0.05 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.06 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.03 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.18 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.19 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.19 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.21 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.21 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.21 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.20 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.02 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.03 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.05 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.05 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.08 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.08 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.08 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.22 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.23 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.23 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.23 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.07 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.19 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.10 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.11 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.23 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.12 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.23 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.25 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.26 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.08 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.14 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.10 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.33 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.06 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.03 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.02 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.02 secs:      72 bytes ==> GET  /
HTTP/1.1 200   2.78 secs:      72 bytes ==> GET  /
HTTP/1.1 200   0.04 secs:      72 bytes ==> GET  /
done.
```

3) Modified java to add the timeOUT in service v2
------------------------------------

Change RecommendationController.java from :

// timeout();

to:

 timeout();

```
cd istio-tutorial/recommendation/java/springboot/src/main/java/com/redhat/developer/demos/recommendation

vi  RecommendationController.java
```

4) Deploy Demo recommendation-v2 with image v3
---------------
```
cd /root/istio-demo/istio-tutorial/recommendation/java/springboot

mvn clean package

docker build -t example/recommendation:v3 .

docker tag example/recommendation:v3 docker-registry.default.svc:5000/tutorial/recommendation:v3

docker login docker-registry.default.svc:5000  -u admin -p $(oc whoami -t)

docker push docker-registry.default.svc:5000/tutorial/recommendation:v3

```


Modify the deployment recommendation-v2 and change image source to docker-registry.default.svc:5000/tutorial/recommendation:v3



5) Check again the service with siege and check the times
--------------------
```
oc project tutorial

oc get route

siege -r 2 -c 20 -v http://customer-tutorial.app.192.168.33.2.xip.io

** SIEGE 3.1.4
** Preparing 20 concurrent users for battle.
The server is now under siege...
HTTP/1.1 200   0.23 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.24 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.26 secs:      73 bytes ==> GET  /
HTTP/1.1 200   3.24 secs:      76 bytes ==> GET  /
HTTP/1.1 200   3.25 secs:      76 bytes ==> GET  /
HTTP/1.1 200   3.26 secs:      76 bytes ==> GET  /
HTTP/1.1 200   3.26 secs:      73 bytes ==> GET  /
HTTP/1.1 200   3.27 secs:      76 bytes ==> GET  /
HTTP/1.1 200   2.27 secs:      73 bytes ==> GET  /
HTTP/1.1 200   3.05 secs:      76 bytes ==> GET  /
HTTP/1.1 200   0.07 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.06 secs:      73 bytes ==> GET  /
HTTP/1.1 200   3.38 secs:      73 bytes ==> GET  /
HTTP/1.1 200   5.31 secs:      77 bytes ==> GET  /
HTTP/1.1 200   6.09 secs:      77 bytes ==> GET  /
HTTP/1.1 200   6.35 secs:      77 bytes ==> GET  /
HTTP/1.1 200   5.35 secs:      73 bytes ==> GET  /
HTTP/1.1 200   5.37 secs:      73 bytes ==> GET  /
HTTP/1.1 200   5.42 secs:      77 bytes ==> GET  /
HTTP/1.1 200   6.16 secs:      77 bytes ==> GET  /
HTTP/1.1 200   2.16 secs:      73 bytes ==> GET  /
HTTP/1.1 200   8.32 secs:      77 bytes ==> GET  /
HTTP/1.1 200   8.37 secs:      77 bytes ==> GET  /
HTTP/1.1 200   3.04 secs:      77 bytes ==> GET  /
HTTP/1.1 200   8.43 secs:      77 bytes ==> GET  /
HTTP/1.1 200   5.20 secs:      77 bytes ==> GET  /
HTTP/1.1 200   3.46 secs:      73 bytes ==> GET  /
HTTP/1.1 200   6.39 secs:      73 bytes ==> GET  /
HTTP/1.1 200  11.40 secs:      77 bytes ==> GET  /
HTTP/1.1 200   5.99 secs:      73 bytes ==> GET  /
HTTP/1.1 200   5.06 secs:      73 bytes ==> GET  /
HTTP/1.1 200   3.10 secs:      73 bytes ==> GET  /
HTTP/1.1 200  11.44 secs:      77 bytes ==> GET  /
HTTP/1.1 200   3.02 secs:      73 bytes ==> GET  /
HTTP/1.1 200   6.09 secs:      77 bytes ==> GET  /
HTTP/1.1 200   0.03 secs:      73 bytes ==> GET  /
HTTP/1.1 200   9.52 secs:      77 bytes ==> GET  /
HTTP/1.1 200   9.51 secs:      77 bytes ==> GET  /
HTTP/1.1 200   6.08 secs:      77 bytes ==> GET  /
HTTP/1.1 200   3.04 secs:      77 bytes ==> GET  /
done.

```
