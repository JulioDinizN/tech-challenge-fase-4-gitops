# ToggleMaster Phase 4 GitOps

Repositório do estado desejado Kubernetes para o Tech Challenge Fase 4. Evolui do repositório GitOps da Fase 3, commit `555ac3f`, e adiciona a stack de observabilidade em `observability/`.

Este repositório não instala Argo CD, não acessa o OKE e não aplica manifests. As políticas de auto-sync estão explicitamente desabilitadas até a infraestrutura, os secrets de bootstrap e os manifests migrados serem revisados.

## Responsabilidades

- Uma base e um overlay homolog por microsserviço e para o Self-Healing.
- Uma tag de imagem independente por serviço.
- Recursos compartilhados em platform/.
- AppProject, ApplicationSet e Application de plataforma em clusters/homolog/.
- Stack de observabilidade (OTel Collector, Prometheus, Loki e Grafana) em observability/.
- Nenhum segredo em texto puro, base64 ou arquivo gerado.

## Fluxo

~~~text
tech-challenge-fase-4 main
  -> CI publica imagens sha-* no OCIR
  -> um job altera os overlays afetados neste repositório
  -> um único commit registra a promoção
  -> Argo CD reconcilia depois que auto-sync for autorizado
~~~

## Estrutura

~~~text
apps/<service>/base/        # cinco serviços + self-healing
apps/<service>/overlays/homolog/
platform/base/
platform/overlays/homolog/
observability/
clusters/homolog/
~~~

Os overlays renderizam 38 recursos: 11 compartilhados, 4 para auth, 4 para flag, 4 para targeting, 5 para evaluation, 4 para analytics e 6 para o Self-Healing (Deployment, Service, ServiceAccount, Role, RoleBinding e SecretProviderClass). A stack de observabilidade fica em `observability/`, com projeto Argo CD próprio. Cada recurso tem um único dono. Identificadores e imagens de bootstrap ainda precisam ser configurados antes da implantação. Apenas homolog será provisionado; produção não é exigida pelo enunciado.

## Segurança

OCI Vault permanece como fonte de segredos. Este repositório poderá conter somente nomes/OCIDs de secrets e endpoints não secretos. O imagePullSecret privado do OCIR será materializado por um mecanismo de bootstrap; seu token nunca será commitado.

## Ativação futura

A habilitação exige revisão explícita de:

1. Manifests renderizados sem placeholders.
2. Vault/CSI e imagePullSecret disponíveis.
3. Imagens sha-* existentes no OCIR.
4. Argo CD instalado pelo state platform.
5. Mudança de automated.enabled para true.
