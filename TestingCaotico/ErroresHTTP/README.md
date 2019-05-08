1) Delete the virtualservice and destinationrule

This configuration will produce an error every 50% of the request, it will allow us to configure and check how infrastructure works until an error.

```
oc create -f destination-rule-recommendation.yml -n tutorial destinationrule.networking.istio.io/recommendation created

oc create -f virtual-service-recommendation-503.yml -n tutorial
virtualservice.networking.istio.io/recommendation created

curl  http://customer-tutorial.app.192.168.33.2.xip.io
customer => 503 preference => 503 fault filter abort

curl  http://customer-tutorial.app.192.168.33.2.xip.io
customer => preference => recommendation v2felix from '5b5f574cc4-wc6f2': 9

curl  http://customer-tutorial.app.192.168.33.2.xip.io
customer => 503 preference => 503 fault filter abort

curl  http://customer-tutorial.app.192.168.33.2.xip.io
customer => preference => recommendation v2felix from '5b5f574cc4-77m2n': 5

```
