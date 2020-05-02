# Kubernetes (on virtual machines) The Hard Way - Deployment

Vagrant + Ansible to deploy a local Kubernetes cluster based on [Kubernetes (on virtual machines) The Hard Way](https://github.com/leonseng/Kubernetes-on-virtual-machines-The-Hard-Way).

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

Run `vagrant up`. That's it!

### Access cluster

The director VM contains the `kubectl` command to operate the cluster. To access the director VM, run `vagrant ssh director`.

## Todo
- Setup API loadbalancer using HAProxy with Keepalived
