# Observabilidade

Estado desejado da stack de monitoramento da Fase 4, reconciliado pelo Argo CD no projeto `observability`.

```text
observability/
  applications/   # AppProject e Applications (charts Helm com versão fixa + values deste repositório)
  values/         # Values do kube-prometheus-stack, Loki e OpenTelemetry Collector
  config/         # Namespace, SecretProviderClasses e ConfigMaps do Grafana (dashboard, alerta, contact points)
```

| Application | Chart | Versão | Sync wave |
| --- | --- | --- | --- |
| observability-config | Kustomize `observability/config` | — | 0 |
| kube-prometheus-stack | prometheus-community/kube-prometheus-stack | 92.14.0 | 1 |
| loki | grafana/loki | 7.5.0 | 1 |
| opentelemetry-collector | open-telemetry/opentelemetry-collector | 0.177.0 | 2 |

- **OTel Collector** (DaemonSet `otel-collector`): recebe OTLP dos serviços, lê os logs de todos os contêineres (`file_log`), adiciona metadados do Kubernetes e exporta métricas ao Prometheus (remote write), logs ao Loki (OTLP) e traces ao New Relic (OTLP).
- **Prometheus**: métricas das aplicações e do cluster. Alertmanager desligado; os alertas são do Grafana.
- **Loki**: single binary com armazenamento local.
- **Grafana**: sidecars carregam o dashboard (`grafana_dashboard: "1"`) e regras/contact points (`grafana_alert: "1"`). Credenciais e chaves de integração vêm do OCI Vault pela SecretProviderClass `grafana-secrets`.

O dashboard e a regra de alerta em `config/grafana/` são a fonte única: o Docker Compose do repositório principal monta os mesmos arquivos.

Placeholders `__OCI_*__` são preenchidos por `scripts/ops/configure-gitops.py` do repositório principal no bootstrap.
