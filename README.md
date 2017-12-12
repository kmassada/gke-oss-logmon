# logomon

```
gcloud beta container clusters create "logging-monitoring" \
--zone "us-central1-a" \
--no-enable-cloud-logging \
--no-enable-cloud-monitoring
```

```
kubectl get pods --all-namespaces | grep fluentd
```

git clone https://github.com/kubernetes/examples.git
cd examples/staging/elasticsearch/

kubectl create namespace logmon-system

inject in metadata
namespace: logmon-system

replace in staging/elasticsearch/es-rc.yaml
- name: es
- name: elasticsearch-logging

kubectl apply -f staging/elasticsearch/service-account.yaml
kubectl apply -f staging/elasticsearch/es-svc.yaml
kubectl apply -f staging/elasticsearch/es-rc.yaml

kubectl create -f staging/elasticsearch/rbac.yaml

$ kubectl get service elasticsearch
$ ES_IP=XXXX
$ curl $ES_IP:9200

wget https://raw.githubusercontent.com/fluent/fluentd-kubernetes-daemonset/master/fluentd-daemonset-elasticsearch.yaml 

edit 
namespace: logmon-system


kubectl apply -f fluentd-daemonset-elasticsearch.yaml 

wget ttps://raw.githubusercontent.com/coreos/blog-examples/master/monitoring-kubernetes-with-prometheus/prometheus.yml

inject
namespace: logmon-system

kubectl get pods -l app=prometheus -o name -n logmon-system | \
	sed 's/^.*\///' | \
	xargs -I{} kubectl port-forward {} 9090:9090
