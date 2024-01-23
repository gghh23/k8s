1. helm repo add prometheus-community  https://prometheus-community.github.io/helm-charts
2. touch values.yaml < EOF 

    prometheus:
    prometheusSpec:
        serviceMonitorSelectorNilUsesHelmValues: false
        serviceMonitorSelector: {}
        serviceMonitorNamespaceSelector: {}

    grafana:
    sidecar:
        datasources:
        defaultDatasourceEnabled: true
    additionalDataSources:
        - name: Loki
        type: loki
        url: http://loki-loki-distributed-query-frontend.monitoring:3100
    EOF


3. kubectl create ns monitoring
4. helm install prom prometheus-community/kube-prometheus-stack -n monitoring --values values.yaml
5. helm repo add grafana https://grafana.github.io/helm-charts
6. touch promtail-values.yaml < EOF

    config:
    clients:
        - url: "http://loki-loki-distributed-gateway/loki/api/v1/push"
    EOF
7. helm upgrade --install loki grafana/loki-distributed -n monitoring --set service.type=LoadBalancer
8. helm upgrade --install promtail grafana/promtail -f promtail-values.yaml -n monitoring
9. kubectl patch service/prom-grafana -n monitroing --type='json' -p '[{"op":"replace","path":"/spec/type","value":"NodePort"}]'
10. cred Garafana 
        Username: admin
        Password: prom-operator
11. helm install nginx-app chart/
12. kubectl apply -f pv-storage-loki-alert.yaml
13. kubectl apply -f pv-storage-loki-0.yaml

        