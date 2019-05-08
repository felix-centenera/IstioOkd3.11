Istio allow tracing of the request with Jaeger.

You can add to your installation with the next steps "--set tracing.enabled=true":


```
helm template install/kubernetes/helm/istio --name istio --namespace istio-system --set tracing.enabled=true | kubectl apply -f -

oc get services

oc expose service tracing -n istio-system

oc get route

NAME                   HOST/PORT                                                   PATH      SERVICES               PORT              TERMINATION   WILDCARD
tracing                tracing-istio-system.app.192.168.33.2.xip.io                          tracing                http-

```

The console will be able: tracing-istio-system.app.192.168.33.2.xip.io

To see trace data, you must send requests to your service. The number of requests depends on Istio’s sampling rate. You set this rate when you install Istio. The default sampling rate is 1%. You need to send at least 100 requests before the first trace is visible. To send a 100 requests to the productpage service, use the following command:
$ for i in `seq 1 100`; do curl -s -o /dev/null http://$GATEWAY_URL/productpage; done

```
for i in `seq 1 100`; do curl -s -o /dev/null http://customer-tutorial.app.192.168.33.2.xip.io; done
```

Sources:

https://istio.io/docs/tasks/telemetry/distributed-tracing/jaeger/
