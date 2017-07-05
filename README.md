# **Ansible Role: Sidekiq Systemd**

An ansible role to setup sidekiq, clockwork etc. as systemd services.

## Installation

ansible-galaxy install amritsingh.sidekiq-systemd

## Requirements

This role only requires Ansible version 1.9+.

## Role Variables

Following are the variables required

```yaml
tmp_storage_dir: "/tmp"              # Temporary storage to store backup of sidekiq configuration
user_name: ubuntu                    # username
app_path: /home/ubuntu/project       # path of the application
sidekiq_env:                         # Environment variabels
  RAILS_ENV: production
```

Path to the file with sidekiq/clockwork processes config has to be specified in variable sidekiq_systemd_conf_file like this:
```yaml
sidekiq_systemd_conf_file: /home/ubuntu/project/config/sidekiq_systemd.yml
```

The file looks like this:
```yaml
---
config:
  - service_name: sidekiq1
    command: "bundle exec sidekiq -c 4 -q default -L log/sidekiq.log"
    options:
      - MemoryLimit=1200M
  - service_name: sidekiq2
    command: "bundle exec sidekiq -c 4 -q default -L log/sidekiq.log"
  - service_name: clockwork
    command: "bundle exec clockwork config/clock.rb >> log/clockwork.log 2>&1
```

## Dependencies

None

## Example Playbook

```yaml
---

- hosts: all
  vars:
    user_name: ubuntu
    app_path: /home/ubuntu/project
    tmp_storage_dir: /home/ubuntu/
    sidekiq_systemd_conf_file: /home/ubuntu/project/config/systemd_services.yml
  roles:
    - role: amritsingh.sidekiq-systemd
