# Exercise 5 – Helm Upgrade Failure

## Objective

Understand why a Helm upgrade can fail when immutable fields are modified and learn the safe approach for upgrading Kubernetes Deployments.

---

## Scenario

A production application was deployed using Helm.

### Version 1

```yaml
selector:
  matchLabels:
    app: payment
```

During an upgrade, the selector was changed to:

### Version 2

```yaml
selector:
  matchLabels:
    app: payment-v2
```

Running the following command:

```bash
helm upgrade payment-service .
```

Resulted in:

```
UPGRADE FAILED:
spec.selector:
field is immutable
```

---

## Why the Upgrade Failed

`spec.selector` is an immutable field in a Kubernetes Deployment.

When a Deployment is created, Kubernetes permanently associates it with Pods matching the selector.

Changing the selector later could cause the Deployment to manage a different set of Pods, so Kubernetes blocks the update to prevent unexpected behavior.

---

## Safe Upgrade Approach

### Recommended Solution

1. Uninstall the existing release.

```bash
helm uninstall payment-service
```

2. Install the updated chart.

```bash
helm install payment-service .
```

### Best Practice

Avoid changing:

- `spec.selector`
- `matchLabels`

Instead, update only:

- Docker image
- Replica count
- Environment variables
- Resource limits
- Configurations

---

## Commands Used

```bash
helm create payment-chart

helm install payment-service .

kubectl get deployment

helm upgrade payment-service .

helm uninstall payment-service

helm install payment-service .
```

---

## Screenshots

### Upgrade Failed

Shows the immutable selector error during Helm upgrade.

- `screenshots/upgrade-failed-immutable-selector.png`

### Reinstall Success

Shows successful deployment after uninstalling and reinstalling the Helm release.

- `screenshots/reinstall-success.png`

---

## Key Learning

- Understood how Helm performs upgrades.
- Learned why `spec.selector` is immutable.
- Reproduced a real production upgrade failure.
- Learned the recommended recovery approach.
- Practiced troubleshooting Kubernetes Deployment upgrade issues.

---

## Technologies Used

- Kubernetes
- Helm
- Minikube
- Ubuntu
