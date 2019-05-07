1) Check the service
--------------------------------------

```
oc project tutorial

oc get route

siege -r 2 -c 20 -v http://customer-tutorial.app.192.168.33.2.xip.io

HTTP/1.1 200   0.29 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.29 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.30 secs:      73 bytes ==> GET  /
HTTP/1.1 200   3.28 secs:      77 bytes ==> GET  /
HTTP/1.1 200   3.29 secs:      77 bytes ==> GET  /
HTTP/1.1 200   3.30 secs:      77 bytes ==> GET  /
HTTP/1.1 200   3.30 secs:      77 bytes ==> GET  /
HTTP/1.1 200   3.02 secs:      77 bytes ==> GET  /
HTTP/1.1 200   3.02 secs:      73 bytes ==> GET  /
HTTP/1.1 200   6.31 secs:      77 bytes ==> GET  /
HTTP/1.1 200   6.31 secs:      77 bytes ==> GET  /
HTTP/1.1 200   6.02 secs:      77 bytes ==> GET  /
HTTP/1.1 200   5.32 secs:      77 bytes ==> GET  /
HTTP/1.1 200   5.33 secs:      73 bytes ==> GET  /
HTTP/1.1 200   6.33 secs:      77 bytes ==> GET  /
HTTP/1.1 200   2.04 secs:      73 bytes ==> GET  /
HTTP/1.1 200   5.34 secs:      73 bytes ==> GET  /
HTTP/1.1 200   5.35 secs:      73 bytes ==> GET  /
HTTP/1.1 200   5.35 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.04 secs:      73 bytes ==> GET  /
HTTP/1.1 200   5.38 secs:      73 bytes ==> GET  /
HTTP/1.1 200   3.09 secs:      73 bytes ==> GET  /
HTTP/1.1 200   3.10 secs:      73 bytes ==> GET  /
HTTP/1.1 200   2.11 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.07 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.10 secs:      73 bytes ==> GET  /
HTTP/1.1 200   9.31 secs:      77 bytes ==> GET  /
HTTP/1.1 200   8.32 secs:      77 bytes ==> GET  /
HTTP/1.1 200   2.01 secs:      73 bytes ==> GET  /
HTTP/1.1 200   8.35 secs:      77 bytes ==> GET  /
HTTP/1.1 200   2.01 secs:      73 bytes ==> GET  /
HTTP/1.1 200   8.37 secs:      77 bytes ==> GET  /
HTTP/1.1 200   3.11 secs:      77 bytes ==> GET  /
HTTP/1.1 200   0.13 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.13 secs:      73 bytes ==> GET  /
HTTP/1.1 200   5.01 secs:      77 bytes ==> GET  /
HTTP/1.1 200   5.00 secs:      77 bytes ==> GET  /
HTTP/1.1 200   3.02 secs:      77 bytes ==> GET  /
HTTP/1.1 200   5.00 secs:      77 bytes ==> GET  /
HTTP/1.1 200   3.09 secs:      77 bytes ==> GET  /
done.

Transactions:		          40 hits
Availability:		      100.00 %
Elapsed time:		       12.46 secs
Data transferred:	        0.00 MB
Response time:		        3.76 secs
Transaction rate:	        3.21 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		       12.08
Successful transactions:          40
Failed transactions:	           0
Longest transaction:	        9.31
Shortest transaction:	        0.04
```

2)Remplace the DestinationRule
---------------------------
```
oc replace -f destination-rule-recommendation_cb_policy_version_v2.yml
```

2) Check the service again with the new-DestinationRule
--------------------------------------

In this case give a 503 when the pod recommendation V2 cannot answer the request.
503 for the user but we not overload the infraestructure.

```
oc project tutorial

oc get route

[root@master-one CanaryRelease]# siege -r 2 -c 20 -v http://customer-tutorial.app.192.168.33.2.xip.io
** SIEGE 3.1.4
** Preparing 20 concurrent users for battle.
The server is now under siege...
HTTP/1.1 200   0.06 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.07 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.10 secs:      73 bytes ==> GET  /
HTTP/1.1 503   0.10 secs:     116 bytes ==> GET  /
HTTP/1.1 200   0.04 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.02 secs:      73 bytes ==> GET  /
HTTP/1.1 503   0.14 secs:     116 bytes ==> GET  /
HTTP/1.1 200   0.09 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.10 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.12 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.13 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.13 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.14 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.15 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.18 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.12 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.10 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.19 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.10 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.08 secs:      73 bytes ==> GET  /
HTTP/1.1 503   0.12 secs:     116 bytes ==> GET  /
HTTP/1.1 503   0.06 secs:     116 bytes ==> GET  /
HTTP/1.1 200   0.02 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.07 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.02 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.02 secs:      73 bytes ==> GET  /
HTTP/1.1 503   0.06 secs:     116 bytes ==> GET  /
HTTP/1.1 200   0.10 secs:      73 bytes ==> GET  /
HTTP/1.1 200   3.05 secs:      77 bytes ==> GET  /
HTTP/1.1 200   2.22 secs:      73 bytes ==> GET  /
HTTP/1.1 200   2.23 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.01 secs:      73 bytes ==> GET  /
HTTP/1.1 200   2.32 secs:      73 bytes ==> GET  /
HTTP/1.1 503   2.39 secs:     116 bytes ==> GET  /
HTTP/1.1 503   0.08 secs:     116 bytes ==> GET  /
HTTP/1.1 200   0.05 secs:      73 bytes ==> GET  /
HTTP/1.1 200   0.02 secs:      73 bytes ==> GET  /
HTTP/1.1 200   6.05 secs:      77 bytes ==> GET  /
HTTP/1.1 200   6.00 secs:      77 bytes ==> GET  /
HTTP/1.1 200   5.00 secs:      77 bytes ==> GET  /
done.

Transactions:		          33 hits
Availability:		       82.50 %
Elapsed time:		       12.06 secs
Data transferred:	        0.00 MB
Response time:		        0.97 secs
Transaction rate:	        2.74 trans/sec
Throughput:		        0.00 MB/sec
Concurrency:		        2.66
Successful transactions:          33
Failed transactions:	           7
Longest transaction:	        6.05
Shortest transaction:	        0.01

```
