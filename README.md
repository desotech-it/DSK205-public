# Desotech DSK205 Files courses

## Kubernetes monitoring and alerting

Kubelet natively exposes cadvisor metrics at https://kubernetes.default.svc:443/api/v1/nodes/{node-name}/proxy/metrics/cadvisor and we can use a prometheus server to scrape this endpoint. These metrics can then be visualized using Grafana. Metrics can alse be scraped from pods and service endpoints if they expose metrics on /metrics (as in the case of nginx-ingress-controller), alternatively you can specify custom scrape target in the prometheus config map.

Some Important metrics which are not exposed by the kubelet, can be fetched using kube-state-metrics and then pulled by prometheus.

## Standalone Prometheus Setup

1. Deployment: 
    - Deploy Alertmanger: `kubectl apply -f /alertmanager`
    - Deploy Prometheus: `kubectl apply -f /prometheus`
    - Deploy Kube-state-metrics: `kubectl apply -f /kube-state-metrics`
    - Deploy Node-Exporter: `kubectl apply -f /node-exporter`
    - Deploy Grafana: `kubectl apply -f /grafana`

2. Once grafana is running:
    - You can now build your custon dashboards or simply import dashboards from grafana.net. Dashboard #315 and #1471 are good to start with.

## Note

1. A Cluster-binding role for prometheus is already being created by the config. The role currently has admin permissions, however you can modify it to a viewer role only.
2. if you need to update the prometheus config, it can be reloaded by making an api call to the prometheus server. `curl -XPOST http://<prom-service>:<prom-port>/-/reload`
3. Some basic alerting rules are defined in the prometheus rules file which can be updated before deploying. You can also add more rules under the same groups or create new ones.
4. Before deploying prometheus please create a pvc Volume of size 250Gi or more.
5. Please update `00-alertmanager-configmap.yaml` to reflect correct api_url for Slack. You can additionally add more receievers. Ref:  https://prometheus.io/docs/alerting/configuration/ 