
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

Day 02 — YAML and Pod creation.

