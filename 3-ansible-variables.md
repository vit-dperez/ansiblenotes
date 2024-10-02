# Ansible Variables

# Variable

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

