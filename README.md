# 🚀 Projeto 02 — Aplicação Java com Kubernetes

Este projeto tem como objetivo demonstrar a execução de uma aplicação Java containerizada rodando em Kubernetes, aplicando boas práticas de configuração, escalabilidade e separação de responsabilidades.

O foco está em simular um ambiente próximo ao real, utilizando recursos fundamentais do Kubernetes como Deployments, Services, ConfigMaps, Secrets e Ingress.

---

## 🧱 Arquitetura do Projeto

* Aplicação Java simples (endpoint HTTP)
* Container Docker
* Kubernetes (Minikube)
* Deployment para gerenciamento de Pods
* Service para exposição interna
* ConfigMap para configurações não sensíveis
* Secret para dados sensíveis
* Ingress para acesso HTTP externo

---

## 📂 Estrutura de Diretórios

```text
project-02-k8s/
├── app/
│   └── (código da aplicação Java)
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   ├── secret.yaml
│   └── ingress.yaml
└── README.md
```

---

## ⚙️ Configurações com ConfigMap

As configurações da aplicação (como mensagens e parâmetros não sensíveis) são gerenciadas via **ConfigMap**, permitindo alterar o comportamento da aplicação **sem necessidade de rebuild da imagem Docker**.

Exemplos de uso:

* Mensagens exibidas pela API
* Variáveis de ambiente não sensíveis

---

## 🔐 Gerenciamento de Segredos com Secret

Dados sensíveis, como tokens e credenciais, são armazenados em **Secrets**, evitando exposição direta no código ou nos manifests Kubernetes.

Esses valores são injetados na aplicação via variáveis de ambiente.

---

## 📦 Deployment e Escalabilidade

A aplicação é gerenciada por um **Deployment**, garantindo:

* Alta disponibilidade
* Recriação automática de Pods em caso de falha
* Facilidade para escalar horizontalmente

Exemplo de escala manual:

```bash
kubectl scale deployment app-deployment --replicas=2
```

---

## 🔗 Service

O **Service** abstrai os Pods e fornece um ponto único de acesso interno à aplicação, garantindo comunicação estável mesmo com múltiplas réplicas.

---

## 🌐 Ingress

O **Ingress** é utilizado para simular um cenário mais próximo de produção, centralizando o acesso HTTP à aplicação e permitindo roteamento por domínio.

---

## 🌐 Como acessar a aplicação

### 🔹 Via Service (debug local)

```bash
kubectl port-forward svc/app-service 8080:80
```

Acesse:

```bash
curl http://localhost:8080
```

Ou pelo navegador:

```
http://localhost:8080
```

---

### 🔹 Via Ingress

1. Obter o IP do Minikube:

```bash
minikube ip
```

2. Adicionar no arquivo `/etc/hosts`:

```text
<IP_DO_MINIKUBE> app.local
```

3. Acessar no navegador:

```text
http://app.local
```

---

## 📊 Logs da Aplicação

Os logs podem ser consultados diretamente via Kubernetes:

```bash
kubectl logs -l app=java-app
```

Os logs foram utilizados durante o projeto para **troubleshooting**, incluindo erros de configuração de image pull, secrets e service.

---

## 🧪 Testes Manuais

A aplicação foi validada utilizando:

* `curl`
* Navegador web
* Port-forward
* Ingress

Todos os testes confirmaram que a aplicação responde corretamente às requisições HTTP.

---

## 🎯 Objetivo do Projeto

Este projeto foi desenvolvido com foco em aprendizado prático e consolidação de conceitos essenciais de Kubernetes, simulando um ambiente real de deploy de aplicações containerizadas.

---

## 👨‍💻 Autor

**Daniel Viana**
GitHub: [https://github.com/danielviana2127](https://github.com/danielviana2127)

