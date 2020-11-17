# Tutorial GCloud Load Balance

## Expondo uma aplicação com ingress:
1 - Vamos criar um deployment com o arquivo: app-deployment.yml
> kubectl apply -f app-deployment.yml 
2 - Vamos criar um service para essa aplicação: app-service.yml
> kubectl apply -f app-service.yml 

Como no gcp a aplicação não é exposta para internet como padrão, mesmo estando como nodePort ele não é acessado externamente.

Para que o aplicativo de servidor da Web HTTP(S) possa ser acessado publicamente, vamos criar um recurso de entrada.
Vamos utilizar o CLoud Loadbalancing da GCP

3 - Vamos criar nosso ingress: ingress.yml
> kubectl apply -f ingress.yml

A aplicação fica acessivel via o Cloud Balancer, para visualizar o ip de acesso: kubectl get ingress
> kubectl get ingress 

## Definindo um ip estático
Vamos agora configurar um ip estático, esse ip gerado é um ip temporário e pode ser perdido depois de um tempo.
Podemos alterar esse ip temporário para estático no painél da GCP na aba de endereços de ip externos.

4 - Ou configurar dinâmicamente na hora da criação do nosso ingress, rode o comando:   
> gcloud compute addresses create web-static-ip --global 
Isso cria um ip estático, vinculado a identificação web-static-ip em modo global

5 - Modifique o ingress.yml acrescentando essa linha no metadata em baixo do name: 
```yml 
  annotations :
    kubernetes.io/ingress.global-static-ip-name: "web-static-ip" 
```
Ficando assim: 
```yml
  apiVersion: networking.k8s.io/v1beta1
  kind: Ingress
  metadata:
    name: basic-ingress
    annotations:
      kubernetes.io/ingress.global-static-ip-name: "web-static-ip"
  spec:
    backend:
      serviceName: web
      servicePort: 8080
```

## Processando várias aplicações no mesmo loadbalancer
Como o serviço de balanceamento de carga é cobrado por balanceador, podemos criar apenas 1 para nosso cluster.

6 - Vamos criar o deployment do 2 app: app2-deployment.yml
> kubectl apply -f app2-deployment.yml

7 - Vamos criar o service do 2 app: app2-service.yml
> kubectl apply -f app2-service.yml

8 - Vamos criar o novo ingress: fanout-ingress.yml

Nele configuramos duas regras, que é todas requisições feitas no path /v2 será no app2 e
todas as outras requisições no app1
> kubectl create -f fanout-ingress.yml
