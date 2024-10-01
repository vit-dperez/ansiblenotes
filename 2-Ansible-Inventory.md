# Ansible Inventory

## Ansible inventory

- Ansible can work with one or multiple systems on the infrastructure at the same time.

- In order to work with multiple servers Ansible needs to establish connectivity to those servers (SSH Linux) (Powershell Windows).

- Agentless: you don't need to install any additional software on the target machines to be able to work with Ansible.

- If you don't provide the machines with an Ansible `inventory` they take the default one at `/etc/ansible/hosts`

- The inventory file is in an INI like format, its simply a number of servers listed one after the other.

**More on inventory files**

- To set an alias in ansible
```
web ansible_host=server1.company.com ansible_connection=ssh
db ansible_host=server2.company.com ansible_connection=winrm
mail ansible_host=server3.company.com ansible_connection=ssh
web2 ansible_host=server4.company.com ansible_connection=winrm

# Work with the localhost
localhost ansible_connection=localhost
```

**Inventory Parameters**:
- ansible_connection - ssh/winrm/localhost
- ansible_port - 22/5986
- ansible_user - root/administrator
- ansible_ssh_pass - Password

**Inventory Groups**

```
[group]
server1
server2
```

- Let us now create a group of groups. Create a new group called `all_servers` and add the previously created groups `web_servers` and `db_servers` under it.

Note: Syntax would be as follows:

```
[all_servers:children]
web_servers
db_servers
```

## Inventory Formats

- If we compare a small start up business and a Multinational company, the size of each one will be different.

- For start-ups the `ini` configuration will be fine, but for multinational we use `yaml`.

**Why do we need different inventory formats**

- Flexibility
- Geographical Location

**Different inventory format types**

- INI: is the simplest most straightforward.

```
[webservers]
web1.example.com
web2.example.com

[dbservers]
db1.example.com
db2.example.com
```

- YAML: is more structured and flexible than the INI format.

```
all:
  children:
    webservers:
      hosts:
        web1.example.com:
        web2.example.com:
    dbservers:
      hosts:
        db1.example.com:
        db2.example.com:
```

