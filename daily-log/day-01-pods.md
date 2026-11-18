
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
# Day 08 — Deployment Rollbacks

**Status:** Completed  
**Date:** 12/11/2026  
**Topic:** Kubernetes Deployments — Rollbacks

## 1. Objective

Understand how Kubernetes manages Deployment revisions and how to revert an application to a previous version using Rollbacks.

## 2. Commands Practiced

### View Deployment revision history

```bash
kubectl rollout history deployment nginx-deployment
```

### Inspect a specific revision

```bash
kubectl rollout history deployment nginx-deployment --revision=2
```

### Rollback to a specific revision

```bash
kubectl rollout undo deployment nginx-deployment --to-revision=1
```

### Rollback to the previous revision

```bash
kubectl rollout undo deployment nginx-deployment
```

### Monitor rollout status

```bash
kubectl rollout status deployment nginx-deployment
```

## 3. Key Learnings

- Kubernetes maintains a history of Deployment revisions.
- Each change to the Pod Template can generate a new revision.
- A rollback creates a new revision instead of simply restoring the old revision number.
- Kubernetes can reuse an existing ReplicaSet during a rollback.
- The `pod-template-hash` identifies the Pod Template, not the Deployment revision.
- The `revisionHistoryLimit` controls how many old ReplicaSets are retained. Its default value is 10.
- Without `--to-revision`, `kubectl rollout undo` returns to the previous revision.

## 4. Practical Lab

Investigated the revision history of `nginx-deployment`.

| Revision | Container Image |
|---|---|
| 1 | nginx |
| 2 | nginx:1.26 |
| 3 | nginx |

Executed a rollback to revision 1:

```bash
kubectl rollout undo deployment nginx-deployment --to-revision=1
```

After the rollback, the Deployment history showed revisions 2 and 3.

An interesting observation was that revision 3 recovered the same `pod-template-hash` observed in revision 1, demonstrating that the Pod Template had returned to its previous configuration.

## 5. Important Concepts

**Deployment → ReplicaSet → Pods**

The Deployment Controller manages ReplicaSets, which are responsible for maintaining the desired number of Pods.

During a rollback, Kubernetes can reactivate an existing ReplicaSet instead of creating an entirely new one.

## 6. Knowledge Check

- What command displays the revision history?
- How can we return to a specific revision?
- Does a rollback create a new revision?
- What is the difference between Revision and Pod Template Hash?
- Can Kubernetes reuse an existing ReplicaSet during a rollback?

## 7. Next Challenge

Study Deployment update strategies:

- RollingUpdate
- Recreate
- maxSurge
- maxUnavailable

Continue practicing Kubernetes administration with a focus on the CKA certification.

# Dia 08 — Deployment Strategies: RollingUpdate e Recreate

**Data:** 16/11/2026  
**Status:** Concluído  
**Trilha:** Kubernetes CKA

## 1. Objetivo

Compreender as estratégias de atualização de Deployments no Kubernetes, analisar o comportamento dos ReplicaSets e praticar atualizações e Rollbacks em laboratório.

## 2. Conteúdos estudados

### RollingUpdate

Estratégia padrão de atualização gradual dos Pods.

- `maxSurge`: quantidade adicional de Pods permitida durante a atualização.
- `maxUnavailable`: quantidade máxima de Pods que podem ficar indisponíveis durante a atualização.
- `maxSurge` utiliza arredondamento para cima.
- `maxUnavailable` utiliza arredondamento para baixo.

### Recreate

Estratégia que encerra os Pods antigos antes de iniciar os novos.

- Não utiliza `maxSurge` ou `maxUnavailable`.
- Pode provocar indisponibilidade temporária da aplicação.
- Evita a coexistência das versões antiga e nova durante a atualização.
- Útil quando versões diferentes da aplicação não podem funcionar simultaneamente.

## 3. Prática no laboratório

Alteração da estratégia utilizando:

```bash
kubectl edit deployment nginx-deployment
```

Configuração aplicada:

```yaml
strategy:
  type: Recreate
```

Atualização da imagem:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.27
```

Nova atualização:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.28
```

Acompanhamento dos recursos:

```bash
kubectl get pods -w
kubectl get replicasets -w
kubectl get deployment nginx-deployment
```

## 4. Principais aprendizados

1. Alterar a estratégia de atualização não provoca, por si só, a recriação dos Pods existentes.
2. Uma alteração no Pod Template dispara uma nova revisão do Deployment.
3. Durante o Recreate, o ReplicaSet antigo é escalado para zero e o novo assume as réplicas desejadas.
4. ReplicaSets antigos são preservados para possibilitar Rollbacks.
5. O Kubernetes pode reutilizar um ReplicaSet existente durante um Rollback, caso o Pod Template correspondente já exista.
6. O estado Terminating merece atenção: um Pod pode continuar encerrando seus processos mesmo após o início da exclusão.

## 5. Comandos importantes para a CKA

| Comando | Finalidade |
|---|---|
| `kubectl edit deployment` | Editar um Deployment |
| `kubectl set image` | Atualizar a imagem |
| `kubectl get replicasets` | Listar ReplicaSets |
| `kubectl get pods -w` | Acompanhar Pods em tempo real |
| `kubectl rollout history` | Consultar revisões |
| `kubectl rollout undo` | Reverter uma atualização |

## 6. Reflexão do dia

Hoje avancei na compreensão do funcionamento interno dos Deployments, principalmente na relação entre estratégias de atualização, ReplicaSets e Rollbacks.

O laboratório permitiu observar como o Kubernetes substitui os Pods e mantém o histórico de versões, reforçando a importância de compreender o mecanismo de reconciliação do cluster.

**Próximo passo:** continuar aprofundando o gerenciamento de Deployments e praticar questões no estilo CKA.

# Dia 7 — Diagnóstico de falha em um rollout

## Objetivo

Simular uma falha de atualização em um Deployment, investigar a causa
e recuperar a versão anterior.

## Prática

Configurei o Deployment para usar RollingUpdate e apliquei uma tag de
imagem inexistente. O novo Pod apresentou os estados `ErrImagePull` e
`ImagePullBackOff`, enquanto os Pods anteriores permaneceram disponíveis
neste teste.

Usei `kubectl describe pod` para consultar os eventos e identificar que a
tag da imagem não foi encontrada. Depois, consultei o histórico e executei
um rollback.

## Comandos utilizados

```bash
kubectl patch deployment nginx-deployment \
  -p '{"spec":{"strategy":{"type":"RollingUpdate"}}}'

kubectl set image deployment/nginx-deployment nginx=nginx:versao-inexistente
kubectl get pods -w
kubectl describe pod <nome-do-pod>
kubectl rollout history deployment/nginx-deployment
kubectl rollout undo deployment/nginx-deployment
kubectl rollout status deployment/nginx-deployment
kubectl get replicasets
