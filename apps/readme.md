Aqui temos a aplicação de exemplo para nosso lab.

Ela é bem simples, mostra uma mensagem em um background que pode ser customizado pelo configmap.

para subir a aplicação:

1. construir a imagem
docker build -t presentation-helm .

2. tag para o repositório do google
docker tag presentation-helm gcr.io/presentation-helm/presentation-helm:v1.0.0

3. subir para o repositório
docker push gcr.io/presentation-helm/presentation-helm:v1.0.0