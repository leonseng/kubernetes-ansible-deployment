# Kubernetes Cluster Deployment via Ansible

Ansible playbook to deploy a local Kubernetes cluster based on [Kubernetes (on virtual machines) The Hard Way](https://github.com/leonseng/Kubernetes-on-virtual-machines-The-Hard-Way).

## Create cluster

To create a standard cluster, run

```
ansible-playbook -i inventory.yaml playbook.yaml
```

To configure HTTP proxy for containerd on the workers, run

```
ansible-playbook -i inventory.yaml playbook.yaml --extra-vars "http_proxy=http://proxy:123"
```