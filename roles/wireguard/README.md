# wireguard role
 
Sets up a WireGuard VPN using Docker (`linuxserver/wireguard` image).
 
- The **server** runs on the machine where you run Ansible.
- A **client** runs on each target host.
## Requirements
 
- Docker on your machine and on the target hosts
- UDP port `51820` open on your machine
## Usage
 
```bash
./setup example_group wireguard start   # set up the VPN
./setup example_group wireguard stop    # remove the containers
```
 
## What `start` does
 
On your machine:
 
1. Enables IP forwarding in the kernel.
2. Starts the `wireguard-server` container. Configs are saved to `roles/wireguard/files/config`.
On each target host:
 
3. Creates `/opt/wireguard`.
4. Copies that host's peer config to `/opt/wireguard/wg0.conf`.
5. Starts the `wireguard-client` container.
## What `stop` does
 
Removes the `wireguard-server` and `wireguard-client` containers. Config files are kept.
 
## Settings
 
Set in `tasks/start-wireguard.yml`:
 
| Setting | Value |
|---------|-------|
| Server address | IP of your machine |
| Port | `51820` |
| Peers | `host1, host2` |
| VPN subnet | `10.13.13.0/24` by default |
 
**Important:** peer names must match your host names in the inventory. If your hosts have different names, change `PEERS`.
