* Remplace VirtualService to repart the traffic 50% 50%

```
istioctl replace -f virtual-service-safari-recommendation-v2.yml  -n tutorial
```
* test with curl

```
curl -A Safari <url>

curl -A Firefox <url>
```

Example:

[root@master-one ControlPorContenido]# curl -A Safari http://customer-tutorial.app.192.168.33.2.xip.io:80
customer => preference => recommendation v2 from '5b5f574cc4-44x9j': 8
[root@master-one ControlPorContenido]# curl -A Safari http://customer-tutorial.app.192.168.33.2.xip.io:80
customer => preference => recommendation v2 from '5b5f574cc4-44x9j': 9
