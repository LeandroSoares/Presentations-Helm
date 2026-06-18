# Acesso ao cluster k3s (VPS)

## Objetivo

Garantir ambiente previsível para demos ao vivo.

## Checklist

- kubeconfig funcional
- namespace `helm-workshop` existente
- permissao para install/upgrade/rollback

## Validacao

```bash
kubectl get nodes
kubectl get ns
kubectl get pods -n helm-workshop
```
