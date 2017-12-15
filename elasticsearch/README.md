# es

https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/fluentd-elasticsearch

## References

### elasticsearch from kubernetes/examples.git

download the elasticsearch files
```
git clone https://github.com/kubernetes/examples.git
cd examples/staging/elasticsearch/
```

create a *-system namespace to manage this
```
kubectl create namespace logmon-system
```

inject in metadata the correct namespace 
```
namespace: logmon-system
```

replace in staging/elasticsearch/es-rc.yaml
```
- name: es
+ name: elasticsearch-logging
```

apply!
```
kubectl apply -f staging/elasticsearch/service-account.yaml
kubectl apply -f staging/elasticsearch/es-svc.yaml
kubectl apply -f staging/elasticsearch/es-rc.yaml
kubectl create -f staging/elasticsearch/rbac.yaml
```

test and verify
```
$ kubectl get service elasticsearch
$ ES_IP=XXXX
$ curl $ES_IP:9200
```
