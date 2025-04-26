# Helm

---

# 🛣️ Roadmap for Mastering Helm in Kubernetes

1. **What is Helm really? Why does it exist?**
2. **Core Concepts**
   - Charts
   - Templates
   - Values
   - Releases
3. **Helm Architecture**
4. **How Helm Talks to Kubernetes**
5. **Hands-on: Creating, Installing, Upgrading, and Rolling Back with Helm**
6. **Deep Dive: Templates, Sprig Functions, and Helm Hooks**
7. **Best Practices: Directory Structure, Versioning, Secrets Management**
8. **Advanced: Helmfile, Helm Secrets, CI/CD with Helm**
9. **Helm v2 vs v3: (Why Tiller was removed)**
10. **Real-world Problems Helm Solves (and Doesn't)**
11. **Security & Limitations**
12. **When NOT to use Helm**

---

# 1. 🧠 What is Helm?

> Helm is like **apt** (for Ubuntu) or **yum** (for CentOS) — but for Kubernetes apps.

In Kubernetes:
- You usually have **multiple YAML files**: Deployment, Service, ConfigMap, Secret, Ingress, etc.
- Installing an app (e.g., Nginx) manually means writing 5+ YAMLs.

💥 **Problem:** 
- Tedious
- Error-prone
- Hard to upgrade consistently

💡 **Solution = Helm**

Helm **packages** all those YAMLs into a **Chart** (a bundle).
- You **install** the Chart.
- Helm **injects variables**.
- Helm **manages upgrades**, **rollback**, and **uninstall**.
- Helm **keeps history** of installs (like Git for your deployments).

---

# 2. 📦 Core Concepts

## ➡️ Charts
A **Chart** is a package of Kubernetes resources.
- Like a `.deb` file in Ubuntu.
- Contains everything needed to deploy an app.

**Chart structure:**
```
mychart/
├── Chart.yaml      # metadata (name, version, etc.)
├── values.yaml     # default config
├── templates/      # Kubernetes YAML files (templated)
├── charts/         # subcharts
├── README.md       # usage instructions
```

---

## ➡️ Templates
- Kubernetes YAML files with Go-template syntax `{{ }}`.
- You replace values dynamically.
- Example:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Values.service.name }}
spec:
  ports:
    - port: {{ .Values.service.port }}
```

## ➡️ Values
- External configuration
- Given by `values.yaml` or during `helm install -f custom-values.yaml`.
- They get injected into templates.

**Example values.yaml:**
```yaml
service:
  name: myservice
  port: 8080
```

## ➡️ Releases
Every time you install a chart, it creates a **Release**:
- `helm install myapp ./mychart`
- Creates release **myapp**.
- Upgrades and rollbacks happen release by release.

---

# 3. 🏛️ Helm Architecture

Helm CLI ↔ Helm Library ↔ Kubernetes API

- No more **Tiller** after Helm v3 (big security fix).
- Helm now directly talks to Kubernetes like kubectl.

Helm stores its metadata in Kubernetes under the `Secret` or `ConfigMap` of the `namespace` where it's deployed.

---

# 4. 🛰️ How Helm Talks to Kubernetes

- When you do `helm install`, Helm:
  1. Loads Chart files.
  2. Renders templates locally.
  3. Sends the final Kubernetes YAML to Kubernetes API.
  4. Kubernetes schedules pods, services, etc.
  5. Helm records everything (metadata).

**Example final rendered file:** (no more `{{ }}`, fully filled out YAMLs)

---

# 5. 🛠️ Hands-On Commands

✅ Install a chart:
```bash
helm install myapp ./mychart
```

✅ Install with custom values:
```bash
helm install myapp ./mychart -f custom-values.yaml
```

✅ Upgrade a release:
```bash
helm upgrade myapp ./mychart -f custom-values.yaml
```

✅ Rollback to previous release:
```bash
helm rollback myapp 1
```

✅ Uninstall:
```bash
helm uninstall myapp
```

✅ List installed releases:
```bash
helm list
```

---

# 6. 🧩 Deep Dive: Templates, Functions, and Hooks

- Templates are written using **Golang text/template** + **Sprig library functions**.
- You can use:
  - `{{ .Values.xyz }}`
  - `{{ if }}`, `{{ else }}`, `{{ range }}`, etc.
  - Sprig functions like `default`, `upper`, `lower`, `repeat`, etc.

**Example Template:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-deployment
spec:
  replicas: {{ .Values.replicaCount }}
  template:
    spec:
      containers:
      - name: {{ .Chart.Name }}
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

---

# 7. 📐 Best Practices

- Keep charts small and modular.
- Use subcharts for dependencies.
- Document your `values.yaml` properly.
- Always template secret generation securely.
- Version your charts (semver: `1.2.3`).

---

# 8. 🛡️ Advanced Topics

- **Helmfile**: Manage many charts at once.
- **Helm Secrets**: Encrypt your `values.yaml` (using SOPS).
- **CI/CD**: Automate `helm upgrade` in pipelines (GitLab, GitHub Actions).

---

# 9. ⚙️ Helm v2 vs v3

- Helm v2 had a server called **Tiller** inside the cluster — a BIG security hole.
- Helm v3 removed Tiller — now Helm is **client-only**.
- Helm v3 = safer + simpler.

---

# 10. 🌍 Real World: Why Helm is Loved

✅ Easy app packaging  
✅ Clean upgrade/rollback  
✅ No manual kubectl YAML madness  
✅ Good for GitOps (store `values.yaml` in Git)  
✅ Huge ecosystem (Bitnami, ArtifactHub)

---

# 11. 🔒 Security and Limitations

- Always audit public charts (they can be dangerous).
- Secrets can be leaked in Helm charts if careless.
- Helm does not enforce app-level security — you must add RBAC, PSPs separately.

---

# 12. ❌ When NOT to Use Helm

- If your app is very simple (single Deployment + Service), plain Kubernetes YAMLs might be enough.
- If you need **extremely custom** Kubernetes logic (like dynamic CRD creation), Helm might feel restrictive.

---

# 🎯 Quick Summary

| Term | Meaning |
|:----|:--------|
| Helm | Kubernetes package manager |
| Chart | App definition bundle |
| Template | YAML with variables |
| Values | External configs |
| Release | Installed instance |

---

