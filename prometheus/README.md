# prometheus

## permisions
kubectl create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin [--user=<user-name>]

## references
https://github.com/prometheus/prometheus/tree/stable/documentation/examples
https://github.com/prometheus/prometheus/issues/2606 
https://github.com/prometheus/prometheus/pull/2641

### prom from coreos/blog-examples.git

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