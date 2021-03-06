# loki-stack
~~~

helm repo add loki https://grafana.github.io/loki/charts
helm repo update

helm upgrade --install loki-stack --namespace=loki-stack loki/loki-stack --set grafana.enabled=true --set prometheus.enabled=true --set   prometheus.server.persistentVolume.enabled=false --set prometheus.alertmanager.persistentVolume.enabled=false

kubectl get secret --namespace loki-stack loki-stack-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

~~~

Edit grafana service and set nodeport or create ingress. 


Variable setting for loki dashboard:
~~~
$namespace	label_values(kube_pod_info, namespace)
$pod	label_values(kube_pod_info{namespace=~"$namespace"}, pod)

or if any issue

$namespace	label_values(namespace)
$pod label_values(container_network_receive_bytes_total{namespace=~"$namespace"}, pod)

Add a query-->{namespace="$namespace", pod=~"$pod"}
      Visualization --> logs
      
~~~

Per query entry increase

~~~
kubectl edit secret loki-stack -n loki-stack
echo 'code'  | base64 --decode

limits_config:
  enforce_metric_name: false
  reject_old_samples: true
  reject_old_samples_max_age: 168h
  max_entries_limit_per_query: 0
   
or from lens edit secret and save   
  ~~~
  
