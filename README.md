# logomon

create a cluster and disable loggin and monitoring

```
gcloud beta container clusters create "logging-monitoring" \
--zone "us-central1-a" \
--no-enable-cloud-logging \
--no-enable-cloud-monitoring
```

verify that there are no fluentd running as deamonsets on your cluster
```
kubectl get pods --all-namespaces | grep fluentd
```

## elastic search

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

## fluentd

get an example
```
wget https://raw.githubusercontent.com/fluent/fluentd-kubernetes-daemonset/master/fluentd-daemonset-elasticsearch.yaml 
```

inject in metadata the correct namespace 
```
namespace: logmon-system
```

create daemonset
```
kubectl apply -f fluentd-daemonset-elasticsearch.yaml 
```

## prometheus

get an example
```
wget https://raw.githubusercontent.com/coreos/blog-examples/master/monitoring-kubernetes-with-prometheus/prometheus.yml
```

inject in metadata the correct namespace 
```
namespace: logmon-system
```
test and verify
```
kubectl get pods -l app=prometheus -o name -n logmon-system | \
	sed 's/^.*\///' | \
	xargs -I{} kubectl port-forward {} 9090:9090 -n logmon-system
```