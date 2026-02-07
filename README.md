# systemd service manager Ansible role

This is an [Ansible](https://www.ansible.com/) role which manages systemd services.


## Features

- **starting** (restarting) services, in order, according to their `priority`. Services can all be stopped cleanly and then started anew, or they can be restarted one-by-one (see `systemd_restart_mode`)

- making services **auto-start** (see `systemd_service_autostart_enabled`)

- **verifying** services managed to start (see `systemd_verify_up_enabled`)

- **stopping** services, in order, according to their `priority`

- starting/stopping all defined services, or a group of services (`--tags=restart-group`, `--tags=stop-group`)

- restarting services by cleanly stopping them and restarting them, or one by one


## Usage

Example playbook:

```yaml
- hosts: servers
  roles:
    - when: systemd_service_manager_enabled | bool
      role: galaxy/com.devture.ansible.role.systemd_service_manager
```

Example playbook configuration (`group_vars/servers` or other):

```yaml
# See `systemd_auto_list` and `systemd_additional_services`
systemd_auto_list: |
  {{
    ([{'name': 'some-service.service', 'priority': 1000}])
    +
    ([{'name': 'another-service.service', 'priority': 1500}])
  }}
```

Example playbook invocations tags (e.g. `ansible-playbook -i inventory/hosts setup.yml --tags=XXXXX`):

- `restart`, `restart-all`, `start-all` - restarts all services and potentially makes them auto-start (depending on `systemd_service_autostart_enabled`)

- `restart-group`, `start-group` - restarts services belonging to the specified group (e.g. `--extra-vars="group=core"`)

- `stop`, `stop-all` - stops all services

- `stop-group` - stops services belonging to the specified group (e.g. `--extra-vars="group=core"`)

