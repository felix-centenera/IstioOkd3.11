1) Replace the VirtualService with the new one.
---------------------------------------------------
In the new VirtualService we are telling that if the pod answer with an error retry for at least 3 times. With this solution, in case one of the pods have been pull out the service in the retry we will go to one of the other.

```
oc replace -f virtual-service-recommendation-v1_and_v2_retry.yml -n tutorial
```

2) Try to see that any user will receive an error.
---------------------------------------------------
```

[root@master-one PoolEjection]# siege -r 2 -c 20 -v http://customer-tutorial.app.192.168.33.2.xip.io

```
