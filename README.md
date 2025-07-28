# Podmortem Helm Charts

This repository distributes the official Helm charts for the **Podmortem** project.

> Podmortem is a Kubernetes-native operator that performs intelligent postmortem analysis of pod failures using pattern matching and AI-powered explanations.

Currently available chart(s):

| Chart | Description |
|-------|-------------|
| `podmortem-operator` | Deploys the Podmortem operator, log-parser, AI-interface, CRDs and supporting PVC into a cluster. |

---

## Prerequisites

* Helm ≥ 3.9
* Kubernetes ≥ v1.22
* Cluster has outbound network access to pull container images (or you pre-load them locally)

---

## Installing from this repository

```bash
# from the root of this repository
helm upgrade --install podmortem ./charts/podmortem-operator \
  --create-namespace -n podmortem-system
```
---

## Quick start

Install the operator into its own namespace:

```bash
helm upgrade --install podmortem ./charts/podmortem-operator \
  --create-namespace \
  -n podmortem-system
```

After a minute you should see three pods running:

```bash
kubectl get pods -n podmortem-system
NAME                                        READY   STATUS    AGE
podmortem-operator-6b976f7b4c-q4jvb         1/1     Running   45s
podmortem-log-parser-service-5c59f9c77c-r6xg4   1/1   Running   45s
podmortem-ai-interface-service-5f6fb78d5f-24mt9  1/1   Running   45s
```

### Customising

All tunables live in `charts/podmortem-operator/values.yaml`. Example:

```bash
helm upgrade --install podmortem podmortem/podmortem-operator \
  --create-namespace -n podmortem-system \
  --set operator.image.tag=v0.2.3 \
  --set aIInterface.enabled=false            # run without AI if offline
```

Key values:

* `installCRDs` – set `false` if your cluster admins install CRDs out-of-band.
* `patternCache.storage` – PVC size for pattern libraries.

---

## Chart development

```bash
# lint templates
helm lint charts/podmortem-operator

# run unit tests (if any)
helm unittest charts/podmortem-operator

# dry-run on a kind cluster
kind create cluster --name podmortem-test
helm upgrade --install podmortem charts/podmortem-operator \
  --create-namespace -n podmortem-system
```

You can later automate publishing to a Helm repository of your choice; this README focuses on local installation.

---

## Contributing

Issues and PRs welcome!  Please run `helm lint` and, if possible, test on Kind or your favorite Kubernetes distro before opening a pull request.

---

## License

Apache 2.0.  See the [LICENSE](./LICENSE) file for details.
