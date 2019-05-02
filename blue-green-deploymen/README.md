* Create DestinationRule
```
istioctl create -f destination-rule-recom-v1-v2.yml -n tutorial
```

* Create VirtualService to send all the traffic to deployment V2
```
istioctl create -f virtual-services-recommendation-v2.yml -n tutorial
```

* Check virtualservices
```
istioctl get virtualservices -n tutorial
```

* Check destinationrules
```
istioctl get destinationrules -n tutorial
```

* Remplace VirtualService to send all the traffic to deployment V1
```
istioctl replace -f virtual-services-recommendation-v1.yml -n tutorial
```
