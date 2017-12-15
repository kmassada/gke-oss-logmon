# fluentd

## references

### fluentd from fluent/fluentd-kubernetes-daemonset.git

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