
In CircuitBreak we have give 503 to user when V2 does not be able to answer. So now we are going to avoid the 503 to user with Pool Ejection. The pods that will not be able to answer the request will be put out of the services until it recovers.

1) Scale up to two the recommendation v2.
---------------------------------------------------

```
oc scale --replicas=2  deployment recommendation-v2
```


2) Force error in one of the recommendation v2.
---------------------------------------------------

V2 Recommendation is ready as an example to force the error with a curl setting the error.

```
oc get pods | grep v2

recommendation-v2-74bb946c69-lmn79   2/2       Running   0          3h
recommendation-v2-74bb946c69-wmrnk   2/2       Running   0          6m

oc rsh recommendation-v2-74bb946c69-wmrnk

  sh-4.2$ curl localhost:8080/misbehave
  Next request to / will return a 503
  sh-4.2$

```

```
curl  http://customer-tutorial.app.192.168.33.2.xip.io
```

3) Now we are going to put out the services this V2 pod that answer with a misbehavior.
---------------------------------------------------
```
oc replace -f destination-rule-recommendation_cb_policy_pool_ejection.yml -n tutorial

[root@master-one PoolEjection]# siege -r 2 -c 20 -v http://customer-tutorial.app.192.168.33.2.xip.io
** SIEGE 3.1.4
** Preparing 20 concurrent users for battle.
The server is now under siege...
HTTP/1.1 200   3.09 secs:      78 bytes ==> GET  /
HTTP/1.1 200   3.11 secs:      78 bytes ==> GET  /
HTTP/1.1 200   3.17 secs:      78 bytes ==> GET  /
HTTP/1.1 200   3.19 secs:      78 bytes ==> GET  /
HTTP/1.1 200   3.46 secs:      78 bytes ==> GET  /
HTTP/1.1 200   6.09 secs:      78 bytes ==> GET  /
HTTP/1.1 200   6.12 secs:      78 bytes ==> GET  /
HTTP/1.1 200   6.18 secs:      78 bytes ==> GET  /
HTTP/1.1 200   6.20 secs:      78 bytes ==> GET  /
HTTP/1.1 200   6.47 secs:      78 bytes ==> GET  /
HTTP/1.1 503   5.52 secs:      54 bytes ==> GET  /
HTTP/1.1 200   8.14 secs:      78 bytes ==> GET  /
HTTP/1.1 200   9.19 secs:      78 bytes ==> GET  /
HTTP/1.1 200   8.21 secs:      78 bytes ==> GET  /
HTTP/1.1 200   8.30 secs:      78 bytes ==> GET  /
HTTP/1.1 200   8.62 secs:      78 bytes ==> GET  /
HTTP/1.1 200  11.17 secs:      78 bytes ==> GET  /
HTTP/1.1 200  11.20 secs:      78 bytes ==> GET  /
HTTP/1.1 200  11.21 secs:      78 bytes ==> GET  /
HTTP/1.1 200   9.04 secs:      71 bytes ==> GET  /
HTTP/1.1 200   8.11 secs:      71 bytes ==> GET  /
HTTP/1.1 200   7.77 secs:      71 bytes ==> GET  /
HTTP/1.1 200   6.06 secs:      71 bytes ==> GET  /
HTTP/1.1 200   6.15 secs:      71 bytes ==> GET  /
HTTP/1.1 200   5.13 secs:      71 bytes ==> GET  /
HTTP/1.1 200   5.79 secs:      71 bytes ==> GET  /
HTTP/1.1 200  11.30 secs:      78 bytes ==> GET  /
HTTP/1.1 200   9.55 secs:      78 bytes ==> GET  /
HTTP/1.1 200  12.01 secs:      78 bytes ==> GET  /
HTTP/1.1 200   4.99 secs:      71 bytes ==> GET  /
HTTP/1.1 200   4.91 secs:      71 bytes ==> GET  /
HTTP/1.1 200   4.59 secs:      72 bytes ==> GET  /
HTTP/1.1 200   3.01 secs:      72 bytes ==> GET  /
HTTP/1.1 200   8.07 secs:      78 bytes ==> GET  /
HTTP/1.1 200   7.74 secs:      78 bytes ==> GET  /
HTTP/1.1 200   1.97 secs:      72 bytes ==> GET  /
HTTP/1.1 200   6.18 secs:      78 bytes ==> GET  /
HTTP/1.1 200   5.45 secs:      78 bytes ==> GET  /
HTTP/1.1 200   5.04 secs:      78 bytes ==> GET  /
HTTP/1.1 200   5.07 secs:      78 bytes ==> GET  /
done.

```

Now during 15 second any request will go to the pod that has answer with a 503
In this case always one user will found the error, then the next user "in our example during 15 seconds" will not found the error.
