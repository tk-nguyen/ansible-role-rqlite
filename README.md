# tk-nguyen.rqlite role

This role will install [rqlite](https://rqlite.io/) onto your VM. By default, when you have 3 or more nodes in your inventory and the number of nodes are odd, it will use [automatic clustering](https://rqlite.io/docs/clustering/automatic-clustering/).

## Role Variables

```yaml
# Specify the rqlite version
rqlite_version: "8.29.2"
# By default, it will detect architecture of your machine
rqlite_architecture: "{% if ansible_machine == 'x86_64' %}amd64{% else %}{{ ansible_machine }}{% endif %}"
rqlite_download_url: >-
  https://github.com/rqlite/rqlite/releases/download/v{{ rqlite_version }}/rqlite-v{{ rqlite_version }}-linux-{{ rqlite_architecture }}.tar.gz

# Origin to add to `Access-Control-Allow-Origin` header
rqlite_allow_origin: "*"
# User and group to run rqlite under
rqlite_user: rqlite
rqlite_group: rqlite
# Where to store data
rqlite_data_dir: "/var/lib/rqlite"
# HTTP port for HTTP traffic
rqlite_http_port: 4001
# Raft port for cluster traffic
rqlite_raft_port: 4002
# rqlite listen IP, defaulted to default outgoing IPv4 address
rqlite_listen_ip: "{{ ansible_default_ipv4['address'] }}"
# Enable basic auth for rqlite
rqlite_enable_auth: false
# Configure users for basic auth
# Remember to include `join` permission for everyone, else rqlite won't form a cluster
# More information: https://rqlite.io/docs/guides/security/#configuring-usernames-and-passwords
rqlite_auth_users:
  - username: "*"
    password: ""
    perms: ["all"]
```

## Example Playbook

```yaml
- hosts: rqlite
  roles:
    - role: tk-nguyen.rqlite
```

## Tests

Install all dependencies in your venv. Then run tests with `molecule test --all` to test all scenarios 

```bash
(venv) $ pip install -r requirements.txt
(venv) $ molecule test --all
```

## License

MIT

