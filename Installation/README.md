
1) Install helm
---------------
* Download your desired version

* * https://github.com/helm/helm/releases

```
wget https://storage.googleapis.com/kubernetes-helm/helm-v2.13.1-linux-amd64.tar.gz
```

* Unpack it

```
tar -zxvf helm-v2.0.0-linux-amd64.tgz
```

Note: Find the helm binary in the unpacked directory, and move it to its desired destination (mv linux-amd64/helm /usr/local/bin/helm. From there, you should be able to run the client: helm help


2) Create & Prepare namespace for ISTIO.
----------------------------------------
```
 oc create namespace istio-system

 oc adm policy add-scc-to-user anyuid -z istio-ingress-service-account -n istio-system

 oc adm policy add-scc-to-user anyuid -z default -n istio-system

 oc adm policy add-scc-to-user anyuid -z prometheus -n istio-system

 oc adm policy add-scc-to-user anyuid -z istio-egressgateway-service-account -n istio-system

 oc adm policy add-scc-to-user anyuid -z istio-citadel-service-account -n istio-system

 oc adm policy add-scc-to-user anyuid -z istio-ingressgateway-service-account -n istio-system

 oc adm policy add-scc-to-user anyuid -z istio-cleanup-old-ca-service-account -n istio-system

 oc adm policy add-scc-to-user anyuid -z istio-mixer-post-install-account -n istio-system

 oc adm policy add-scc-to-user anyuid -z istio-mixer-service-account -n istio-system

 oc adm policy add-scc-to-user anyuid -z istio-pilot-service-account -n istio-system

 oc adm policy add-scc-to-user anyuid -z istio-sidecar-injector-service-account -n istio-system

 oc adm policy add-scc-to-user anyuid -z istio-galley-service-account -n istio-system

 oc adm policy add-scc-to-user anyuid -z istio-security-post-install-account -n istio-system

```

Note: A service account that runs application pods needs privileged security context constraints as part of sidecar injection:

```
oc adm policy add-scc-to-user privileged -z default -n <target-namespace>
```
Example:

```
oc adm policy add-scc-to-user privileged -z default -n tutorial
```

* Webhook and certificate signing requests support must be enabled for automatic injection to work. Modify the master configuration file on the master node for the cluster as follows:

** By default, the master configuration file can be found in /etc/origin/master/master-config.yaml.

```
cp -p master-config.yaml master-config.yaml.prepatch

oc ex config patch master-config.yaml.prepatch -p "$(cat master-config.patch)" > master-config.yaml

master-restart api

master-restart controllers

```


Install ISTIO
--------------

curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.1.4 sh -

cd istio-1.1.4

export PATH=$PWD/bin:$PATH

```
helm template install/kubernetes/helm/istio-init --name istio-init --namespace istio-system | kubectl apply -f -

helm template install/kubernetes/helm/istio-init --name istio-init --namespace istio-system | kubectl apply -f -

kubectl get crds | grep 'istio.io\|certmanager.k8s.io' | wc -l

```

Last commando should be 53

```
helm template install/kubernetes/helm/istio --name istio --namespace istio-system | kubectl apply -f -
```

istio-system should be reacheable from all the projects. (Depend on the NetworkPolicy)

```
oc adm pod-network make-projects-global istio-system
```

verifying:
```
kubectl get svc -n istio-system
kubectl get pods -n istio-system
```

Sources:
--------
https://www.paradigmadigital.com/dev/jugando-con-istio-the-next-big-thing-en-microservicios-1-2/?utm_source=Site&utm_campaign=Related_Posts_Right&utm_medium=Blog
https://www.paradigmadigital.com/dev/jugando-con-istio-the-next-big-thing-en-microservicios-2-2/
https://blog.openshift.com/evaluate-istio-openshift/
https://istio.io/docs/setup/kubernetes/install/helm/
https://github.com/helm/helm/blob/master/docs/install.md


helm template install/kubernetes/helm/istio --name istio --namespace istio-system --set tracing.enabled=true | kubectl apply -f -
