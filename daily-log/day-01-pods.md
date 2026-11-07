
# Day 01 — Understanding Kubernetes Pods

**Date:** November 4, 2026

**Status:** Completed

**Study time:** 15 minutes

## 🎯 Objective

Understand Kubernetes Pods and their relationship with containers.

## 💻 Commands practiced

```bash
kubectl get nodes
kubectl get pods -A
kubectl get pods -A -o wide
kubectl describe pod
```

## 🧠 Key learnings

A Pod can run one or more containers.

All containers inside the same Pod share:

- The same IP address.
- The same network namespace.
- Network connectivity through localhost.

The Pod receives an IP address, regardless of how many containers it runs.

Containers in the same Pod must also avoid port conflicts.

## 🏆 Knowledge Check

**Question:** What happens when a Pod runs two containers?

**Answer:** Both containers share the same Pod IP and can communicate through localhost.

## 🔍 Practical results

All commands were successfully executed in the Kubernetes laboratory.

The conceptual knowledge check was answered correctly.

## 💡 Personal reflection

I reinforced my understanding of Kubernetes networking and learned that the Pod, rather than each individual container, owns the network identity.

## Next challenge

# Dia 02 — YAML e criação de Pods

## Objetivo
Compreender a estrutura dos arquivos YAML utilizados pelo Kubernetes e aprender a criar recursos de forma declarativa.

## Conteúdos estudados

- Diferença entre `kind` e `apiVersion`.
- Estrutura de um manifesto YAML.
- Especificação de imagens de containers utilizando o campo `image`.
- Funcionamento do comando `kubectl apply -f`.
- Diferença entre criar Pods diretamente e utilizar Deployments.
- Conceito de réplicas (`replicas`) em Deployments.

## Principais aprendizados

**1. kind e apiVersion**

- `kind`: define o tipo de recurso que será criado, como Pod, Deployment ou Service.
- `apiVersion`: identifica o grupo e a versão da API utilizada pelo recurso.

Exemplos:

```yaml
apiVersion: v1
kind: Pod

# Dia 03 - Deployments e ReplicaSets

🚀 Mais um passo na preparação para a certificação CKA!

Hoje foi dia de praticar **Deployments, ReplicaSets e o mecanismo de reconciliação do Kubernetes**.

No laboratório, testei o escalonamento de um Deployment de 3 réplicas para 0 e, posteriormente, para 3, observando como o Kubernetes mantém o estado desejado da aplicação.

Mais um aprendizado consolidado na prática!

**Próximo desafio:** Rolling Updates e Rollbacks. 💻

#Kubernetes #CKA #DevOps #CloudComputing #LearningByDoing

## Day 04 — Kubernetes Rolling Updates

**Conteúdo estudado:**
- Estratégia de atualização RollingUpdate.
- Como o Deployment Controller utiliza ReplicaSets para substituir Pods gradualmente.
- Parâmetros `maxSurge` e `maxUnavailable`.
- Regras de arredondamento dos percentuais desses parâmetros.
- Relação entre disponibilidade da aplicação e atualização de versões.

**Laboratório prático:**
- Inspeção de Deployments e ReplicaSets.
- Atualização da imagem do Nginx de `nginx:1.25` para `nginx:1.26`.
- Acompanhamento do processo com `kubectl rollout status`.
- Análise do comportamento dos ReplicaSets durante a atualização.

**Configurações estudadas:**

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 0
```

**Próximo desafio:**
- Estudar Rollbacks.
- Consultar o histórico de revisões com `kubectl rollout history`.
- Executar rollback com `kubectl rollout undo`.

**Status:** Rolling Updates — Estudado e praticado.

