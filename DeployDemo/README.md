

1)Install mvn & JDK(not cover in this Readme)
----------------------------------------------
```
wget http://apache.uvigo.es/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz

tar -xvf apache-maven-3.6.1-bin.tar.gz

mkdir -p /usr/local/apache-maven

mv apache-maven-3.6.1 /usr/local/apache-maven/

export M2_HOME=/usr/local/apache-maven/apache-maven-3.6.1

export M2=$M2_HOME/bin

export MAVEN_OPTS=-Xms256m -Xmx512m

export PATH=$M2:$PATH

mvn -version

```

2) Deploy Demo customer
---------------

```
cd /root/istio-demo/istio-tutorial/customer/java/springboot

mvn clean package

docker build -t example/customer:v1 .

docker tag example/customer:v1 docker-registry.default.svc:5000/tutorial/customer:v1

docker push docker-registry.default.svc:5000/tutorial/customer:v1

kubectl apply -f <(istioctl kube-inject -f ../../kubernetes/Deployment.yml) -n tutorial

kubectl create -f ../../kubernetes/Service.yml -n tutorial

```

Modify the deployment customer and change image source to docker-registry.default.svc:5000/tutorial/customer:v1

NOTE:

after i manually add privileged to proxy-init, it works. because selinux is enforcing......this option is needed when inject.

So I modified the deployment for customer and add "privileged: true" for image istio-init.

3) Deploy Demo preferences
---------------
```
cd /root/istio-demo/istio-tutorial/preference/java/springboot

mvn clean package

docker build -t example/preference:v1 .

docker tag example/preference:v1 docker-registry.default.svc:5000/tutorial/preference:v1

docker push docker-registry.default.svc:5000/tutorial/preference:v1

kubectl apply -f <(istioctl kube-inject -f ../../kubernetes/Deployment.yml) -n tutorial

kubectl create -f ../../kubernetes/Service.yml -n tutorial

```

Modify the deployment preference-v1 and change image source to docker-registry.default.svc:5000/tutorial/preference:v1

4) Deploy Demo recommendation-v1
---------------
```
cd /root/istio-demo/istio-tutorial/recommendation/java/springboot

mvn clean package

docker build -t example/recommendation:v1 .

docker tag example/recommendation:v1 docker-registry.default.svc:5000/tutorial/recommendation:v1

docker push docker-registry.default.svc:5000/tutorial/recommendation:v1

kubectl apply -f <(istioctl kube-inject -f ../../kubernetes/Deployment.yml) -n tutorial

kubectl create -f ../../kubernetes/Service.yml -n tutorial

```

Modify the deployment recommendation-v1 and change image source to docker-registry.default.svc:5000/tutorial/recommendation:v1


5) Deploy Demo recommendation-v2
---------------
```
cd /root/istio-demo/istio-tutorial/recommendation/java/springboot/src/main/java/com/redhat/developer/demos/recommendation

vi RecommendationController.java (Change v1 for v2)

cd /root/istio-demo/istio-tutorial/recommendation/java/springboot

mvn clean package

docker build -t example/recommendation:v2 .

docker tag example/recommendation:v2 docker-registry.default.svc:5000/tutorial/recommendation:v2

docker push docker-registry.default.svc:5000/tutorial/recommendation:v2

kubectl apply -f <(istioctl kube-inject -f ../../kubernetes/Deployment-v2.yml) -n tutorial

```

Modify the deployment recommendation-v2 and change image source to docker-registry.default.svc:5000/tutorial/recommendation:v2


Sources:
--------
https://github.com/istio/istio/issues/8387
https://github.com/redhat-developer-demos/istio-tutorial/blob/master/documentation/modules/ROOT/pages/2deploy-microservices.adoc
