# AutomationTool
 
A small Ansible tool. One playbook and one script let you start or stop services on your hosts.
 
## Roles
 
| Role | What it does |
|------|--------------|
| [wireguard](roles/wireguard/README.md) | WireGuard VPN (server on your machine, clients on the hosts) |
| [zabbix](roles/zabbix/README.md) | Zabbix monitoring (server on your machine, agents on the hosts) |
 
## Requirements
 
- Ansible
- Docker
- Remote hosts
- SSH access and root (sudo) rights
## Usage
 
```bash
./setup <HOSTS> <ROLE> <ACTION>
```
 
- `HOSTS` - a group or host from the inventory
- `ROLE` - a service that need to manage
- `ACTION` - action on the service

## Project layout
 
```
ansible.cfg     # Ansible settings
playbook.yml    # runs the chosen role on the chosen hosts
setup           # script to execute ansible playbooks
inventory/      # hosts, groups and their variables
vault/          # password files for ansible-vault
roles/          # services
```
 
## Inventory
 
Edit `inventory/inventory.yml` and replace the example names with your own:
 
```yaml
example_group:
  hosts:
    example_host:
```
 
Variables go in `inventory/group_vars/<group>/` and `inventory/host_vars/<host>/`:
 
- `vars.yml` - normal variables
- `vault.yml` - secret variables (encrypt them with ansible-vault)
## Secrets
 
`ansible.cfg` reads the vault password from `vault/example.txt`. To encrypt a file:
 
```bash
ansible-vault encrypt --encrypt-vault-id example inventory/group_vars/example_group/vault.yml
```
