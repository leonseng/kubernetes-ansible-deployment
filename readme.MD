# Kubernetes (on virtual machines) The Hard Way - Deployment

Ansible playbook to deploy a local Kubernetes cluster based on [Kubernetes (on virtual machines) The Hard Way](https://github.com/leonseng/Kubernetes-on-virtual-machines-The-Hard-Way).

## Getting Started

There are 4 VMs in this deployment:
- director
- controller
- 2 workers

### Prerequisites

The following software are required for this deployment:
- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)

### Create VMs

Run `vagrant up`.

### Create cluster

1. SSH to the director VM by running `vagrant ssh director`
1. Navigate into the `/vagrant/k8s` directory
1. To create a standard cluster, run

    ```
    ansible-playbook -i inventory.yaml playbook.yaml
    ```

    To configure HTTP proxy for containerd on the workers, run

    ```
    ansible-playbook -i inventory.yaml playbook.yaml --extra-vars "http_proxy=http://proxy:123"
    ```