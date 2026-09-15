# Helm Guide – Kubernetes Deploy, Upgrade & Rollback

## Introduction
Helm is a package manager for Kubernetes that helps you define, install, upgrade,
and rollback applications in a simple and reliable way.

Instead of managing multiple Kubernetes YAML files manually, Helm bundles them
into a single package called a **Helm Chart**.

---

## Problems with Traditional Kubernetes Deployments

When deploying applications using `kubectl apply`, teams often face these issues:

- Multiple YAML files (Deployment, Service, ConfigMap, Secret, PVC)
- Correct apply order is required (Namespace → ConfigMap → Deployment)
- Separate YAML files needed for different environments (dev, prod)
- No easy way to track changes
- Rollbacks are manual and error-prone

---

## What is Helm?

Helm is a Kubernetes package manager that:
- Uses **charts** to define applications
- Uses **templates** with placeholders
- Uses **values.yaml** for configuration
- Maintains **release history**
- Supports **easy rollback**

---

## What is a Helm Chart?

A Helm Chart is a collection of files that describe a Kubernetes application.

A typical chart structure:

```
my-chart/
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   └── _helpers.tpl
```

### Chart Components
- **Chart.yaml** – Chart metadata (name, version, description)
- **values.yaml** – Default configuration values
- **templates/** – Kubernetes resource templates

---

## Helm Templates

Helm templates are Kubernetes YAML files with placeholders.
Values are injected from `values.yaml`.

Example snippet:
```
replicas: {{ .Values.replicaCount }}
image: {{ .Values.image.repository }}:{{ .Values.image.tag }}
```

---

## Helm Install

To install an application using Helm:

```
helm install my-app ./my-chart
```

Helm will:
- Render templates
- Apply correct resource order
- Create all Kubernetes resources
- Store release history

---

## Helm Upgrade

To apply changes (image, replicas, configmap, etc.):

```
helm upgrade my-app ./my-chart
```

Helm automatically:
- Detects changes
- Applies updates
- Stores a new revision

---

## Helm List

To view all Helm releases:

```
helm list
```

---

## Helm Rollback

If something goes wrong, rollback to a previous version:

```
helm rollback my-app 1
```

This restores the application to the specified revision instantly.

---

## Release History

Helm stores every install and upgrade as a **revision**.
You can view history using:

```
helm history my-app
```

---

## Public Helm Charts

Many tools provide public Helm charts, such as:

- Prometheus
- Grafana
- Alertmanager
- Loki
- Jaeger / Tempo

Using Helm charts, complex systems can be installed with a single command.

---

## Benefits of Helm

- Simplified deployments
- Environment-specific configuration
- Built-in versioning
- Fast rollbacks
- Standardized application packaging

---

## Conclusion

Helm makes Kubernetes application management easier, safer, and more scalable.
It is an essential tool for DevOps engineers working with Kubernetes.





----------------
Concept	:

Chart	A packaged app — the whole folder of templates + values

Values	The settings that get plugged into the templates

Release	One installed instance of a chart running in your cluster (you can install the same chart twice with different names/settings)

------------

# See what Kubernetes YAML this actually generates (no cluster needed)
helm template myapp ./myapp

# Change a value without touching any files
helm template myapp ./myapp --set replicaCount=5

# Actually deploy it (needs a real cluster)
helm install myapp ./myapp
