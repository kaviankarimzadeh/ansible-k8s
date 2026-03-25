# Kubernetes Multi-Master Cluster Deployment with Ansible

Deploy and manage highly-available Kubernetes clusters using Ansible. This project supports both single-master and multi-master topologies, with HAProxy load-balancing and Keepalived VIP for HA control planes.

## Features

- **Flexible topology**: Single-master or multi-master deployments via simple toggle
- **HA control plane**: HAProxy + Keepalived on master nodes (multi-master mode)
- **Multi-OS support**: Debian/Ubuntu and RedHat/CentOS families
- **Best practices**: Organized roles, idempotent tasks, fact-based OS detection
- **Easy scaling**: Add nodes to inventory, rerun playbooks

## Project Structure

```
.
├── README.md                    # This file
├── site.yml                     # Top-level playbook entry point
├── inventory/
│   ├── hosts.yml               # Host definitions and host variables
│   └── group_vars/
│       ├── all.yml             # Cluster-wide variables
│       ├── k8s_masters.yml      # Master node group variables
│       └── k8s_workers.yml      # Worker node group variables
└── roles/
    └── common/                  # Shared tasks for all nodes
        └── tasks/
            ├── main.yml         # Role entry point
            └── update-packages.yml
```

## Quick Start

### 1. Configure Inventory

Edit `inventory/hosts.yml` with your node IPs and hostnames. Set `keepalived_priority` for each master (higher = takes VIP first).

### 2. Test Connectivity

Verify Ansible can reach all nodes:

```bash
ansible -i inventory/hosts.yml k8s_cluster -m ping
```

### 3. Dry-Run the Playbook

Test what would change without making changes:

```bash
ansible-playbook -i inventory/hosts.yml site.yml --check --diff
```

### 4. Run the Playbook

Deploy the cluster:

```bash
ansible-playbook -i inventory/hosts.yml site.yml
```

## Common Commands

| Command | Purpose |
|---------|---------|
| `ansible-playbook -i inventory/hosts.yml site.yml` | Deploy |
| `ansible-playbook -i inventory/hosts.yml site.yml --check --diff` | Dry-run with diffs |
| `ansible-playbook -i inventory/hosts.yml site.yml -v` | Verbose output |
| `ansible-playbook -i inventory/hosts.yml site.yml --limit k8s-master01` | Run on specific host |
| `ansible -i inventory/hosts.yml k8s_cluster -m ping` | Test connectivity |


## run init command

`kubeadm init --config /etc/kubernetes/kubeadm-init.yml --upload-certs`
`kubeadm join --config /etc/kubernetes/kubeadm-join.yml`
## License

MIT
