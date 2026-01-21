# Projeto 02 – Docker + Kubernetes na Prática

## 🎯 Objetivo

Executar a aplicação do **Projeto 01** em um cluster Kubernetes local, utilizando **Deployment, Service, ConfigMap, Secret e Ingress**, seguindo boas práticas de separação de responsabilidades, segurança e observabilidade básica.

O foco deste projeto é demonstrar **entendimento real do funcionamento do Kubernetes**, indo além de apenas "fazer rodar".

---

## 🧱 Stack utilizada

* Kubernetes (Minikube)
* kubectl
* Docker Hub
* NGINX Ingress Controller

---

## 📁 Estrutura do projeto

```
project-02-k8s/
├── README.md
└── k8s/
    ├── deployment.yaml
    ├── service.yaml
    ├── configmap.yaml
    ├── secret.yaml
    └── ingress.yaml
```

---

## 🔄 Fluxo do request

1. O cliente acessa a aplicação (via Service ou Ingress)
2. O **Service (ClusterIP)** recebe a requisição
3. O Service encaminha para um dos **Pods** do Deployment
4. O container expõe a aplicação na porta **8080**
5. A resposta retorna ao cliente

```
Cliente → Service → Pod → Aplicação Java
```

---

## 📦 Recursos Kubernetes e decisões técnicas

### 🔹 Deployment

Responsável por:

* Criar e gerenciar os Pods
* Garantir alta disponibilidade
* Permitir escalabilidade horizontal

A imagem da aplicação é obtida diretamente do **Docker Hub**, simulando um ambiente real de produção.

---

### 🔹 Service

O Service do tipo **ClusterIP** é utilizado para:

* Expor os Pods internamente no cluster
* Permitir comunicação estável entre recursos
* Viabilizar testes via `kubectl port-forward`

Sem o Service, não é possível acessar a aplicação nem via port-forward nem via Ingress.

---

### 🔹 ConfigMap

Utilizado para armazenar configurações **não sensíveis**, como:

* Porta da aplicação
* Variáveis de ambiente gerais

🔁 Permite alterar configurações **sem rebuild da imagem Docker**.

---

### 🔹 Secret

Utilizado para armazenar **dados sensíveis**, como tokens e credenciais.

* Nenhuma variável sensível está hardcoded no código
* Secrets são injetados como variáveis de ambiente no container

---

### 🔹 Ingress

O Ingress foi configurado para simular acesso externo à aplicação, utilizando o **NGINX Ingress Controller**.

Mesmo com o teste principal sendo feito via Service, o Ingress demonstra entendimento do fluxo de entrada em ambientes Kubernetes.

---

## 📈 Escalabilidade

Para escalar a aplicação horizontalmente:

```bash
kubectl scale deployment app-deployment --replicas=3
```

O Service distribui automaticamente as requisições entre os Pods disponíveis.

---

## 🔧 Alterar configuração sem rebuild

Para alterar uma configuração:

1. Edite o arquivo `configmap.yaml`
2. Aplique novamente:

```bash
kubectl apply -f k8s/configmap.yaml
```

3. Reinicie o Deployment:

```bash
kubectl rollout restart deployment app-deployment
```

Nenhuma nova imagem Docker é necessária.

---

## 📜 Logs da aplicação

Os logs podem ser acessados diretamente via Kubernetes:

```bash
kubectl logs -l app=java-app
```

Isso permite observar o comportamento da aplicação sem acessar diretamente os containers.

---

## 🧪 Testes realizados

A aplicação foi validada utilizando **Service + port-forward**:

```bash
kubectl port-forward svc/app-service 8080:80
curl http://localhost:8080
```

Resposta esperada:

```
🚀 API Java rodando com CI/CD completo!
```

---

## ✅ Critérios atendidos

* ✔ Aplicação rodando em Kubernetes
* ✔ Uso correto de Deployment, Service, ConfigMap, Secret e Ingress
* ✔ Variáveis sensíveis protegidas
* ✔ Configuração desacoplada da imagem
* ✔ Logs acessíveis via kubectl
* ✔ README explicando decisões técnicas

---

## 👨‍💻 Autor

**Daniel Viana**
GitHub: [https://github.com/danielviana2127](https://github.com/danielviana2127)

