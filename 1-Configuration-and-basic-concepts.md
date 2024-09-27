# Ansible

## Why Ansible?

- Its a powerful IT automation tool that we can learn quickly, yet powerful enough to automate even the most complex deployments.

- Scripts were replaced with Ansible Playbooks.

- Simple use case: Ansible is helful when we got a set of servers and we want to restart them in a particular order (shutdown webservers first, then dbs servers and start dbs first and then the web servers), we can write an Ansible Playbook to get this done in a matter of minutes.

- Complex use case: we want to manage a private and public cloud (Amazon and VMWare) and we want to configure applications, and setting up communication between them, manage files, setting firewall-rules, etc.


- The Ansible documentation is hosted at: 
- [AnsibleDocs]: docs.ansible.com

## Ansible Configuration Files

They are stored at: `/etc/ansible/ansible.cfg` 

**Sections**:

- [defualts]: Conifgurations like default configuration path, etc.

Example:

```
[defaults]

inventory       = /etc/ansible/hosts
log_path        = /var/log/ansible.log

library         = /usr/share/my_modules/
roles_path      = /etc/ansible/roles
action_plugins  = /usr/share/ansible/plugins/action

gathering       = implicit

# SSH timeout
timeout         = 10
forks           = 5
```

Ansible will look for the first for the environment variable `ANSIBLE_CONFIG`