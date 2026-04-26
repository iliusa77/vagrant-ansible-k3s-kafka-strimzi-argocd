# 🚀 DevOps Pet Project: Kafka Platform on K3s with GitOps

## 📌 Overview

This project demonstrates a production-like DevOps platform built locally using:

- Vagrant  
- Ansible  
- K3s  
- Apache Kafka (via Strimzi)  
- ArgoCD  

The goal is to simulate a real-world DevOps environment with:
- Infrastructure automation  
- Kubernetes orchestration  
- Event-driven architecture  
- GitOps-based deployments  

---

## 🧱 Architecture

[Vagrant VMs]
   ↓
[Ansible Provisioning]
   ↓
[K3s Cluster (1 master + 2 workers)]
   ↓
[Strimzi Kafka Cluster]
   ↓
[ArgoCD GitOps]
   ↓
[Producer → Kafka → Consumer]

---

## 📁 Project Structure

.
├── infra/
│   ├── Vagrantfile
│   ├── inventory.ini
│   ├── playbook.yml
│   └── roles/
│       ├── common/
│       ├── k3s-master/
│       └── k3s-worker/
│
├── platform/
│   ├── kafka/
│   │   └── helm/
│   ├── apps/

---

## ⚙️ Prerequisites

Install locally:

- VirtualBox  
- Vagrant  
- Ansible  
```bash
- kubectl  
```

---

## 🚀 Deployment Guide

### 1. Start Virtual Machines

cd infra  
vagrant up  

---

### 2. Provision Infrastructure

ansible-playbook -i inventory.ini playbook.yml  

---

```bash
### 3. Configure kubectl
```

vagrant ssh master  

sudo cp /etc/rancher/k3s/k3s.yaml /home/vagrant/  
sudo chown vagrant:vagrant /home/vagrant/k3s.yaml  
export KUBECONFIG=~/k3s.yaml  

```bash
kubectl get nodes  
```

---


### 4. Install Kafka & Strimzi (Automated)

Kafka and Strimzi are now deployed using Helm and automated via Ansible:

1. The Ansible playbook provisions the cluster and installs Strimzi Kafka Operator using the Helm chart in `platform/kafka/helm`.
2. Custom Kafka resources (clusters, topics) can be managed in `platform/kafka/`.

Manual Helm install example (if needed):

```bash
helm repo add strimzi https://strimzi.io/charts/
helm repo update
helm install strimzi-kafka-operator strimzi/strimzi-kafka-operator -n kafka --create-namespace
# Then apply your Kafka CRs as needed
```

---

### 5. Install ArgoCD (Automated)

ArgoCD is installed automatically by the Ansible playbook. No manual steps required.

---

### 6. Access ArgoCD

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443  
```

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o yaml  
```

Open: https://localhost:8080  

---

### 7. Deploy via GitOps

```bash
kubectl apply -f platform/argocd/application.yaml  
```

---

## 🔍 Validation

```bash
kubectl get pods -A  
```

```bash
kubectl get kafka -n kafka  
kubectl get kafkatopic -n kafka  
```

```bash
kubectl logs deployment/producer  
kubectl logs deployment/consumer  
```

---

## 🧹 Cleanup

vagrant destroy -f  

---

## 🎯 Summary

This project demonstrates:
- Infrastructure as Code (Ansible)  
- Kubernetes deployment (K3s)  
- Kafka via Strimzi  
- GitOps via ArgoCD  
