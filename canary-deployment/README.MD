# Tutorial GCloud Canary Deployment

## Criando o Cluster no GCP
Antes de tudo, instale o kubectl e gcloud.

1. Defina o projeto e a zona que deseja.
> gcloud config set project project-id
> gcloud config set compute/zone compute-zone

2. Crie o cluster
> gcloud container clusters create canarydeploymentcluster

3. Caso já tenha um cluster ou criou via interface no GCP.
> gcloud container clusters get-credentials [Nome do Cluster]

## Instalando Prometheus e Grafana via Helm

1. Listar qual a versão mais recente do prometheus:
> helm search repo prometheus

2. Instalar a versão mais recente:
> helm install meu-prometheus --version=11.12.1 stable/prometheus

3. Listar qual a versão mais recente do grafana:
> helm search repo prometheus

4. Instalar a versão mais recente:
> helm install meu-grafana --version=5.5.7 stable/grafana
> Senha do grafana: HsjRDsfOjvxoY2OXSiLQKN6bPXo8BNi1bra8S3bC

# Configurando o Grafana

1. Exponha o pod:
>> kubectl --namespace default port-forward $POD_NAME 3000

2. Crie um data source do prometheus.

3. Crie uma Dashboard para visualizar as requests de um service.
Query Prometheus:
``` sum(rate(nginx_http_requests{app="app"}[5m])) by (version) ```

# Deploy do APP v1

1. Faça o deploy do App v1 localizado na pasta app no arquivo app-v1.yml
> kubectl create -f app/app-v1.yml

2. Execute o service desse App v1 que está localizado na pasta app no arquivo service-app.yml
> kubectl create -f app/service-app.yml

3· Deploy do app v2
> kubectl create -f app/app-v2-canary.yml

- Basta acessar a aplicação e visualizar o dashboard no grafana.

# Exclusão do Cluster

1. Delete o cluster criado
> gcloud container clusters delete canarydeploymentcluster
