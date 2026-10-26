# Observabilidade

Estado desejado da stack de monitoramento da Fase 4, reconciliado pelo Argo CD.

| Componente | Função |
| --- | --- |
| OpenTelemetry Collector | Recebe OTLP dos serviços e roteia métricas, logs e traces |
| Prometheus | Armazenamento e consulta de métricas |
| Loki | Centralização dos logs dos contêineres |
| Grafana | Dashboards, consulta de logs e alertas |

Pendente:

- Applications Argo CD com versões fixas dos charts Helm.
- AppProject ou permissões para os repositórios de charts, o namespace `observability` e os recursos de escopo de cluster (CRDs, ClusterRoles, webhooks).
- Dashboard customizado do Grafana e regras de alerta versionados.
- Inclusão das novas raízes no validador e no workflow de validação.
