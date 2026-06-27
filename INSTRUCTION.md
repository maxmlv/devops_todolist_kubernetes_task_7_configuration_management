# Deploying ConfigMap and Secret

## 1. Deploy

```bash
kubectl apply -f .infrastructure/configmap.yml
kubectl apply -f .infrastructure/secret.yml
kubectl apply -f .infrastructure/deployment.yml
```

---

## 2. Validate

Confirm the ConfigMap and Secret exist:

```bash
kubectl get configmap todoapp-config -n todoapp
kubectl get secret todoapp-secret -n todoapp
```

Confirm the pods picked up the new env vars and restarted cleanly:

```bash
kubectl get pods -n todoapp
```

Check that `PYTHONUNBUFFERED` and `SECRET_KEY` are actually set inside a running container:

```bash
kubectl exec -it <pod-name> -n todoapp -- env | grep -E "PYTHONUNBUFFERED|SECRET_KEY"
```

`PYTHONUNBUFFERED` should show as `1`, and `SECRET_KEY` should show the decoded plaintext value (not the base64 string from the Secret manifest) — confirming Kubernetes correctly decoded it before injecting it into the container.