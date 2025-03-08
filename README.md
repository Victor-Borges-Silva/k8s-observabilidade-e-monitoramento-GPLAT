# **Observabilidade & Mnitoramento -GPLAT- com Kubernetes**
Repositório destinado ao projeto de Observabilidade e Monitoramento usando Grafana, Prometheus,Loki, Alloy e Tempo usando Kubernetes como provedor do serviço

#### Requisitos
Antes de iniciar, você precisará ter as seguintes ferramentas instaladas:

-Docker: Para rodar o Kind.
-Kubectl: Para interagir com o Kubernetes.
-Kind: Para criar e gerenciar o cluster Kubernetes local.
-Helm (opcional): Para facilitar a instalação de pacotes como o Grafana.

## **Passos para Configuração**

### 1. Criando arquivo do cluster Local com Kind
Primeiro, vamos iniciar o cluster Kubernetes local utilizando o Kind.

**kind-config.yaml**
Este arquivo contém a configuração do Kind para a criação do cluster local. Ele define o nome do cluster e os detalhes sobre o seu ambiente.

#### 2. Criando o Cluster
Para criar o cluster, execute o seguinte comando:
```yaml
kind create cluster --config kind/kind-config.yaml
```

#### 3. Verificando o Cluster
Para verificar se o cluster foi criado corretamente, execute:
```yaml
kubectl cluster-info --context kind-k8s-gplat
```

### **Subindo Serviço**

#### 4. Criando o Persistent Volume para o Grafana
A configuração do Grafana requer um PersistentVolumeClaim (PVC) para armazenar dados (**grafana-volume.yaml**). Para criar o PVC, execute:
```yaml
kubectl apply -f grafana-volume.yaml
```

#### 5. Criando o Deployment do Grafana
Agora, vamos criar o Deployment do Grafana.

**grafana-deployment.yaml**
Este arquivo define a configuração do deployment do Grafana. Para aplicar o deployment, execute:
```yaml
kubectl apply -f grafana-deployment.yaml
```

#### 6. Expondo o Grafana com um Service
Para acessar o Grafana, você precisa criar um Service para expô-lo.

**grafana-service.yaml**
Este arquivo define o service para o Grafana. Para aplicar o service, execute:
```yaml
kubectl apply -f grafana-service.yaml
```

### Acessando os Servoços

#### 2 Acessando o Grafana
Se você estiver usando o LoadBalancer no serviço, o IP externo será atribuído assim que o Kubernetes concluir a configuração. Você pode acessar o Grafana através do IP externo. Se o **EXTERNAL-IP** estiver pendente (como no seu caso), use port-forward para acessar localmente:

Para verificar o status do serviço:
```yaml
kubectl get svc -n monitoramento
```

```yaml
kubectl port-forward <pod-name> > 3000:3000 -n monitoramento 
```

