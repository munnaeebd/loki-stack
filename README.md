# loki-stack
~~~

helm repo add loki https://grafana.github.io/loki/charts
helm repo update

helm upgrade --install loki-stack --namespace=loki-stack loki/loki-stack --set grafana.enabled=true --set prometheus.enabled=true --set   prometheus.server.persistentVolume.enabled=false

kubectl get secret --namespace loki-stack loki-stack-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

~~~

Edit grafana service and set nodeport or create ingress. 
