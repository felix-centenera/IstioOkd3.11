* Create DestinationRule
```
istioctl create -f destination-rule-recom-v1-v2.yml -n tutorial
```

* Create VirtualService to repart the traffic 90% 10%
```
istioctl create -f virtual-services-recommendation-v1-v2-90-10.yml -n tutorial
```

* Check virtualservices
```
istioctl get virtualservices -n tutorial
```

* Check destinationrules
```
istioctl get destinationrules -n tutorial
```

* Remplace VirtualService to repart the traffic 50% 50%
```
istioctl replace -f virtual-services-recommendation-v1-v2-50-50.yml  -n tutorial
```
