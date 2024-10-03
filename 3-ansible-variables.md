# Ansible Variables

## Variables

- Stores information that varies with each host

**inventory**
```
web ansible_host=server1.company.com ansible_connection=ssh
db ansible_host=server2.company.com ansible_connection=winrm
mail ansible_host=server3.company.com ansible_connection=ssh
web2 ansible_host=server4.company.com ansible_connection=winrm
```

**Playbook.yml**
```
-
  name: Add DNS server to resolv.conf
  hosts: localhost
  vars:
    dns_server: 10.1.250.10
  tasks:
    - lineinfile:
      path: /etc/resolv.conf
      line: 'nameserver {{ dns_server }};
```

```
---
- hosts: localhost
  vars:
    car_model: 'BMW M3'
    country_name: USA
    title: 'Systems Engineer'
  tasks:
    - command: 'echo "My car is {{ car_model }}"'
    - command: 'echo "I live in the {{ country_name }}"'
    - command: 'echo "I work as a {{ title }}"'
```

- We can also have variables defined in a seperate file dedicated for variables.

```
-
  name: Set Firewall Configurations
  hosts: web
  tasks:
  - firewalld:
      service: https
      permanent: true
      state: enabled

  - firewalld:
      port: '{{ http_port }}'/tcp
      permanent: true
      state: disabled

  - firewalld:
      port: '{{ snmp_port }}'/udp
      permanent: true
      state: disabled

  - firewalld:
      source: '{{ inter_ip_range }}'/24
      Zone: internal
      state: enabled
```

```
# Sample Inventory File

web http_port=8081 snmp_port=161-162 inter_ip_range=192.0.2.0
```

```
# Sample variable File - web.yml

http_port: 8081
snmp_port: 161-162
inter_ip_range: 192.0.2.0
```

- The format used to declare variables in Ansible playbooks is called Jinja2 Templating.

- When using variables with Jinja2 Templating enclose them within quotes if its in the begining of a sentence, else would not be required.

## String Variables

- String variables in Ansible are sequences of characters.

- They can be defined in a playbook, inventory, or passed as command line arguments.

`username: "admin"`

## Number Variables

- Number variables in Ansible can hold integer or floating-point values.

- They can be used in mathematical operations.

`max_connections: 100`

## Boolean Variables

- Boolean variables in Ansible can hold either true or false.

- They are often used in conditional statements.


`debug_mode: true`

| Valid values | Description |
|-------------:|:------------|
| True, 'true', 't', 'yes', 'y', 'on', '1', 1, 1.0 | Truthy values |
| False, 'false', 'f', 'no', 'n', 'off', '0', 0,0.0 | False values |

## List Variables

- List variables in Ansible can hold an ordered collection of values.

- The values can be of any type.

```
packages:
  - nginx
  - postgresql
  - git
```

- Use double quotes to specify them on the playbooks `"{{ packages }}"`.

- To select an item from the list do: `{{ packages[0] }}`

## Dictionary Variables

- Dictionary variables in Ansible can hold a collection of key-value pairs.

- The keys and values can be of any type.


```
user:
  name: "admin"
  password: "secret"
```
- Example: `msg: "Username:{{ user.name }}" Password: {{ user.password }}`


## Variable Precedence

- When Ansible Playbook runs it first associates the group variables and then associates the host variables.

- The variable at the host level takes precedence over the variable defined in the group variable.

- Whatever is defined in the play level overrides the host and group variables.

- If the variable is passed in the command line it takes that varibale.

- If we want to pass the output of a command to another command we can use the `register` directive in the playbook and specify the variable on the second command with the same name.

```
---
- name: Check /etc/hosts file
  hosts: all
  tasks:
  - shell: cat /etc/hosts
    register: result

  - debug:
      var: result

- name: Play2
  hosts: all
  tasks:
  - debug:
      var: result.stdout
```


- We can also get the output of a command using the `-v` option in the command-line.
```
- name: Check /etc/hosts file
  hosts: all
  tasks:
  - shell: cat /etc/hosts
```
```
ansible-playbook -i inventory playbook.yml -v
```

## Variable Scopes

- A scope defines the accesability or the visibility of a variable.

- A variable is accessible depending on how and where it is defined.

## Variable Scopes - Host

- The fist scope is the `hosts scope`.

```
web1 ansible_host=172.20.1.100
web2 ansible_host=172.20.1.101 dns_server=10.5.5.4
web3 ansible_host=172.20.1.102
```

## Variable Scopes - Play

```
---
- name: Play1
  hosts: web1
  vars:
    ntp_server: 10.1.1.1
  tasks:
  - debug:
      var: ntp_server
- name: Play2
  hosts: web1
  tasks:
  - debug:
      var: ntp_server
```

## Variable Scopes - Global

- Passing variables in the command line makes it available thoughout the playbook execution.

```
ansible-playbook playbook.yml --extra-vars "ntp_server=10.1.1.1"
```

## Magic Variables

- Sometimes we need to access a `host` variable that is only defined in one of the hosts.

- There's where Magic variables come handy.

```
web1 ansible_host=172.20.1.100
web2 ansible_host=172.20.1.101 dns_server=10.5.5.4
web3 ansible_host=172.20.1.102
```

**Get the variables from the `web2` host**

```
---
- name: Print dns server
  hosts: all
  tasks:
  - debug:
      msg: '{{ hostvars['web2'].dns_server }}'
```

```
msg: '{{ hostvars['web2'].ansible_host }}'
```

```
msg: '{{ hostvars['web2'].ansible_facts.architecture }}'
```

```
msg: '{{ hostvars['web2'].ansible_facts.devices }}'
```

## Ansible Facts

- When ansible runs a playbook it collects information from the hosts (like ips, configurations, settings), these information are known as `facts` in Ansible.

- The `setup` module that runs automatically by Ansible.

- The `facts task` runs without being specified before the tasks defined in the playbook.

- This information is stored in a variable `ansible_facts`.

```
---
- name: Print hello message
  hosts: all
  tasks:
  - debug:
      var: ansible_facts
```

**If we want to skip the fact's gathering**
```
---
- name: Print hello message
  hosts: all
  gather_facts: no
  tasks:
  - debug:
      var: ansible_facts
```

- The gathering option can be configured in `/etc/ansible/ansible.cfg`:

```
# plays will gather facts by default, which contain information about
# smart - gather by default, but don't regather if already gathered
# implicit - gather by default, turn off with gather_facts: False
# explicit - do not gather by default, must say gather_facts: True
gathering       = implicit
```

- Ansible only gathers facts against hosts that are part of the playbook.


