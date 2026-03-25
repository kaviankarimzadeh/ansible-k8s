# Kubernetes Multi-Master Cluster Deployment with Ansible

Deploy and manage highly-available Kubernetes clusters using Ansible. This project supports both single-master and multi-master topologies, with HAProxy load-balancing and Keepalived VIP for HA control planes.

## Features

- **Flexible topology**: Single-master or multi-master deployments
- **Load balancing**: HAProxy + Keepalived for on-prem; cloud LB support (Hetzner, AWS, etc.)
- **Multi-OS support**: Debian/Ubuntu and RedHat/CentOS families
- **Production-ready**: Idempotent tasks, fact-based OS detection, best practices

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

### 1. Configure Inventory & Load Balancer Mode

Edit `inventory/hosts.yml` with your node IPs and hostnames.

In `inventory/group_vars/all.yml`, set:
- `lb_type: cloud` for external load balancers (Hetzner LB, AWS NLB, etc.) — skips HAProxy/Keepalived
- `lb_type: haproxy` for on-prem HA with HAProxy + Keepalived on masters

Then set `k8s_api_vip` to your load balancer IP or VIP address.

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
