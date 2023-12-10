1.) Install Helm.

2.) Install Grafana operator and the grafana dashboard:

```
kubectl create ns monitoring
```

```
kubectl apply -f https://raw.githubusercontent.com/Pablo-Wynistorf/kubernetes-apps/main/grafana/grafana.yaml
```

```yaml
helm upgrade -i grafana-operator oci://ghcr.io/grafana-operator/helm-charts/grafana-operator --version v5.4.2 --namespace monitoring
```

The default login is:

username: admin

PW: password

Its recommended to change that 😄

When unsing EKS and ELB Ingresscontroller add this to the config file:

```yaml
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: grafana-ingress
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS": 443}]'
    alb.ingress.kubernetes.io/ssl-redirect: '443'
    alb.ingress.kubernetes.io/certificate-arn: <cert arn>
    alb.ingress.kubernetes.io/ssl-policy: ELBSecurityPolicy-2016-08
spec:
  ingressClassName: alb
  rules:
    - host: <domain>
      http:
        paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: grafana
              port:
                number: 80
```

3.) Install Prometheus using Helm.

```yaml
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

```
helm repo update
```

```
helm install prometheus prometheus-community/kube-prometheus-stack --set grafana.enabled=false  --set prometheus.prometheusSpec.additionalScrapeConfigs[0].job_name="namespace-scrape" --set prometheus.prometheusSpec.additionalScrapeConfigs[0].kubernetes_sd_configs[0].role="pod" -n monitoring
```

You can connect to Grafana locally with this command:

```
kubectl port-forward svc/grafana-service -n monitoring 3000
```

You can now visit [http://localhost:3000](http://localhost:3000) to get to the grafana dashboard.
