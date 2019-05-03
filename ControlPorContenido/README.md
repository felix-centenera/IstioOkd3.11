* Remplace VirtualService to repart the traffic 50% 50%

```
istioctl replace -f virtual-service-safari-recommendation-v2.yml  -n tutorial
```
* test with curl

```
curl -A Safari <url>

curl -A Firefox <url>
```
