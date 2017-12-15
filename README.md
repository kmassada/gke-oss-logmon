# logomon

create a cluster and disable loggin and monitoring

```
gcloud beta container clusters create "logging-monitoring-3" \
--zone "us-central1-a" \
--no-enable-cloud-logging \
--no-enable-cloud-monitoring \
--cluster-version "1.8.4-gke.0"
```

verify that there are no fluentd running as deamonsets on your cluster
```
kubectl get pods --all-namespaces | grep fluentd
```

create a *-system namespace to manage this
```
kubectl create namespace logmon-system
```
## fluentd

cd fluentd/
kubectl apply -f rbac.yml
kubectl apply -f configmap.yml
kubectl apply -f fluent-es-ds.yml

## elasticsearch

cd elasticsearch/
kubectl apply -f rbac.yml
kubectl apply -f elasticsearch.yml
kubectl apply -f service.yml

## kibana

cd kibana/
kubectl apply -f kibana.yml
kubectl apply -f service.yml


## prometheus

cd prometheus/
kubectl apply -f rbac.yml
kubectl apply -f prometheus.yml