# Aplicações - App1 e App2

Este repositório contém duas aplicações simples escritas em Phyton utilizando o framework Flask. O objetivo principal é empacotar essas aplicações como imagens Docker e disponibilizá-las em um cluster Kubernetes.

---

📁 Estrutura de Diretórios

```
Apps/
├── app1/
│   ├── Dockerfile
│   ├── requirements.txt
│   └── app.py
└── app2/
    ├── Dockerfile
    ├── requirements.txt
    └── app.py
```

Tanto `app1` quanto `app2` possuem exatamente a mesma estrutura e configuração. Foram mantidas juntas no mesmo repositório por simplicidade e reutilização.

---

Sobre a Aplicação

Cada aplicação consiste em um único endpoint (`/`) que retorna um texto simples. Essa abordagem minimalista permite testar:

- Deploy em Kubernetes
- Exposição via Ingress
- Balanceamento e divisão de tráfego
---

## `app.py`

Aplicação Python simples utilizando Flask:

- Cria uma aplicação Flask com uma rota `/` que retorna um texto.
- O `host='0.0.0.0'` permite que a aplicação seja acessada de fora do container.
- A porta 5000 é a padrão do Flask.

---

## `requirements.txt`

Lista os pacotes necessários para a aplicação. No caso:

```
Flask==2.0.1
```

Este arquivo foi utilizado localmente para instalar as dependências no build da imagem Docker. Está presente no repositório apenas para dar contexto ao Dockerfile.

---

## `Dockerfile`

**Explicação**:

- **python:3.9-alpine**: imagem leve e segura baseada em Alpine Linux.
- **apk add**: instala ferramentas de compilação necessárias para o Flask funcionar corretamente.
- **pip install**: instala as dependências Python da aplicação.
- **CMD**: define o comando padrão de execução da aplicação.

---

## Separação dos Manifestos Kubernetes

Os manifestos do Kubernetes (Deployments, Services, Ingress, etc.) **não estão neste repositório**. Foram mantidos separados para:

- Evitar criação de novas **tags da aplicação** quando há apenas mudanças nos manifestos.
- Melhor organização e **independência entre infraestrutura e aplicação**.
- Facilitar a criação de **esteiras CI/CD desacopladas**.

---

## 🚀 Deploy Manual (Sem CI/CD)

Como ainda **não há pipeline de CI**, os comandos abaixo foram utilizados manualmente para build e push das imagens no ECR:

```bash
# Login no ECR
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 577125335739.dkr.ecr.us-east-1.amazonaws.com

# Build e push App 1
docker build -t app1 ./app1
docker tag app1:latest 577125335739.dkr.ecr.us-east-1.amazonaws.com/app1
docker push 577125335739.dkr.ecr.us-east-1.amazonaws.com/app1

# Build e push App 2
docker build -t app2 ./app2
docker tag app2:latest 577125335739.dkr.ecr.us-east-1.amazonaws.com/app2
docker push 577125335739.dkr.ecr.us-east-1.amazonaws.com/app2
```
