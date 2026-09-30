# zabbix role

Sets up Zabbix monitoring using Docker.

- The **server** (Zabbix server, web UI, MySQL) runs on the machine where you run Ansible.
- An **agent** runs on each target host and sends data to the server.

## Requirements

- Docker with Compose v2 on your machine
- Docker on the target hosts
- Free ports on your machine: `10051`, `8080`, `10050`

## Setup

Fill in two files in `roles/zabbix/files/` before starting.

`.env` (server):

```env
MYSQL_ROOT_PASSWORD=
MYSQL_DATABASE=
MYSQL_USER=
MYSQL_PASSWORD=
ZBX_HOSTNAME=
```

`.env-remote` (agents):

```env
ZBX_HOSTNAME=
ZBX_SERVER_HOST=
ZBX_SERVER_ACTIVE_HOST=
```

Set `ZBX_SERVER_HOST` and `ZBX_SERVER_ACTIVE_HOST` to the IP of your machine. Note that `.env-remote` is copied to every host as is.

## Usage

```bash
./setup example_group zabbix start   # start server and agents
./setup example_group zabbix stop    # stop and remove them
```

After start, open `http://<your machine IP>:8080`. The default login is `Admin` / `zabbix`. Change the password right away.

## What `start` does

On your machine: starts the stack from `files/compose.yml` (server, web, agent, MySQL).

On each target host:

1. Creates `/opt/zabbix`.
2. Copies `.env-remote` to `/opt/zabbix/.env`.
3. Starts the `zabbix-agent` container.

## What `stop` does

Stops the stack on your machine and removes the agent container on the hosts.
