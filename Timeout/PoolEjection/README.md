
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
[root@master-one CanaryRelease]# curl recommendation.tutorial.svc:8080
recommendation v2felix from '74bb946c69-lmn79': 91
[root@master-one CanaryRelease]# curl recommendation.tutorial.svc:8080
recommendation v1 from '7766dff6c6-7g4nw': 496
[root@master-one CanaryRelease]# curl recommendation.tutorial.svc:8080
recommendation v2felix from '74bb946c69-lmn79': 92
[root@master-one CanaryRelease]# curl recommendation.tutorial.svc:8080
recommendation misbehavior from '74bb946c69-wmrnk'
[root@master-one CanaryRelease]# curl recommendation.tutorial.svc:8080
recommendation v1 from '7766dff6c6-7g4nw': 497
[root@master-one CanaryRelease]# curl recommendation.tutorial.svc:8080
recommendation v1 from '7766dff6c6-7g4nw': 498
[root@master-one CanaryRelease]# curl recommendation.tutorial.svc:8080
recommendation misbehavior from '74bb946c69-wmrnk'
[root@master-one CanaryRelease]# curl recommendation.tutorial.svc:8080
recommendation v1 from '7766dff6c6-7g4nw': 499
[root@master-one CanaryRelease]# curl recommendation.tutorial.svc:8080
recommendation v2felix from '74bb946c69-lmn79': 93
[root@master-one CanaryRelease]# curl recommendation.tutorial.svc:8080
recommendation misbehavior from '74bb946c69-wmrnk'
```

3) Now we are going to put out the services this V2 pod that answer with a misbehavior.
---------------------------------------------------
