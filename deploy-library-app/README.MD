#Intro

- Configura o Gclould com o kubectl
``` gcloud container clusters get-credentials primeiro-exemplo --zone us-central1-c ```

- Vamos criar um deployment do redis master, o arquivo yaml está localizando em: ./redis-deployment.yml
``` kubectl create -f redis-deployment.yml ```

- Vamos criar um service para expor nosso pod, o arquivo yaml está localizado em: ./redis-service.yml
``` kubectl create -f redis-service.yml ```

- Nessa parte temos o redis configurado, porém para garantir alta disponibilidade podemos configurar instâncias slaves dele.

- Vamos criar um deployment do redis slave.
  - Lembrando que ele se refencia ao master pela enviroment "GET_HOSTS_FROM"
``` kubectl create -f redis-slave-deployment.yml ```

- Vamos criar um service para expor nosso pod, o arquivo yaml está localizado em: ./redis-slave-service.yml
``` kubectl create -f redis-slave-service.yml ```

- Vamos criar um deployment para o frontend que está localizado em: ./frontend-deployment.yml
``` kubectl create -f frontend-deployment.yml ```

- Vamos criar um service para expor nosso frontend, o arquivo yaml está localizado em: ./redis-slave-service.yml
``` kubectl create -f frontend-service.yml ```

