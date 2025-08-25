# Kubernetes Usando Kind e NGINX Ingress
## Comando para criar estrutura de pastas e arquivos
```bash
mkdir -p ./{cluster,apps/{page1,page2,page3},manifest/{deployments,services,ingress}} && \
touch cluster/kind-config.yaml && \
touch apps/page1/{index.html,Dockerfile} && \
touch apps/page2/{index.html,Dockerfile} && \
touch apps/page3/{index.html,Dockerfile} && \
touch manifest/deployments/{page1-deployment.yaml,page2-deployment.yaml,page3-deployment.yaml} && \
touch manifest/services/{page1-service.yaml,page2-service.yaml,page3-service.yaml} && \
touch manifest/ingress/ingress.yaml && \
echo 'Project structure created successfully!' && \
tree . 2>/dev/null || find . -type f | sor
```
## Comando de criação do cluster
```bash
kind create cluster --config=cluster/kind-config.yaml
```
## Instalar nginx-ingress-controller
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

## Realizar build das imagens das aplicações
```bash
docker build -t page1:latest apps/page1
docker build -t page2:latest apps/page2
docker build -t page3:latest apps/page3
```
## Carregar imagens para o cluster k8s
```bash
kind load docker-image page1:latest --name ingress-cluster
kind load docker-image page2:latest --name ingress-cluster
kind load docker-image page3:latest --name ingress-cluster
```
## Criando deployments das aplicações
```bash
kubectl apply -f manifest/deployments/page1-deployment.yaml
kubectl apply -f manifest/deployments/page2-deployment.yaml
kubectl apply -f manifest/deployments/page3-deployment.yaml
```
## Criando services das aplicações
```bash
kubectl apply -f manifest/services/page1-service.yaml
kubectl apply -f manifest/services/page2-service.yaml
kubectl apply -f manifest/services/page3-service.yaml
```
## Criando ingress da aplicação
```bash
kubectl apply -f manifest/ingress/ingress.yaml
```
##
Apos os passos acima já possivel testar a aplicação na porta 8080