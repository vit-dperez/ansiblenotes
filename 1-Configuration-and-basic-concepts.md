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

## Ansible Configuration Variables

- Ansible will look first for the environment variable `ANSIBLE_CONFIG` to look for the configurations followed by the ansible.cfg file in the current directory from which the Ansible playbooks are run followed by the .ansible.cfg in the user's home directory and lastly the default Ansible config file.

- If we need to change only one variable in one of the other files we can change it as an environment variable, right before we run the ansible playbook set it as: `ANSIBLE_VARNAME`.


Single run:
```
ANSIBLE_GATHERING=explicit ansible-playbook playbook.yml
```

Until exit shell
```
export ANSIBLE_GATHERING=explicit
```
```
ansible-playbook playbook.yml
```

Long-live change
Create a Ansible config file in the Playbooks directory and update the parameter in it.

/opt/web-playbooks/ansible.cfg
```
gathering     = explicit
```

## View Configuration


- List all configurations
```
ansible-config list
```

- Shows the current config gile
```
ansible-config view
```

- Shows the current (comprehensive list) settings
```
ansible-config dump
```

## What is YAML?

- A YAML file is used to represent data

```
Servers:
  - name: Server1
    owner: John
    created: 12232012
    status: active
```

- Key Value Pair
```
Fruit: Apple
Vegetable: Carrot
Liquid: Water
Meat: Chicken
```

- Array/Lists
```
Fruits:
- Orange
- Apple
- Banana

Vegetables:
- Carrot
- Cauliflower
- Tomato
```

- Dictionary/Map
```
Banana:
  Calories: 105
  Fat: 0.4 g
  Carbs: 27 g

Grapes:
  Calories: 62
  Fat: 0.3 g
  Carbs: 16 g
```