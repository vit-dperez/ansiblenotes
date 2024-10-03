# Ansible Playbooks

## Ansible Playbook

- Playbook: a single YAML gile
  - Play: Defines a set of activities (tasks) to be run on hosts
    - Task: An action to be performed on the host
      - Execute a command.
      - Run a script.
      - Install a package
      - Shutdown/Restart

`playbook.yml`
```
-
  name: Play 1
  hosts: localhost
  tasks:
    - mame: Execute command 'date'
      command: date

    - name: Execute script on server
      script: test_script.sh

    - name: Install httpd service
      yum:
        name: httpd
        state: present

    - name: Start web server
      service:
        name: httpd
        state: started
```

- To change the `hosts` value in the playbook we need to specify the host in the inventory first and match the same name on the playbook.

- If a group is specified the listed tasks will be executed on all the hosts listed under that group simultaneously.

### Module

- The `command`, `script`, `yum`, `service` directives under the tasks are called `modules`.

- The information of those modules and more is in the Ansible documentation website.

### Run

- Finally, once the Ansible playbook is successfully build, how do we run it?

- Execute Ansible playbook command
```
ansible-playbook playbook.yml
```

- Syntax: ansible-playbook <playbook file name>
```
ansible-playbook --help
```

## Verifying Playbooks

- Verifying a playbook before executing it in a production environment is a crucial practice.

- Its like a rehersal before the performance allowing us to catch and rectify any error or unexpected behaviors in controled _environments_.

- Verifying the playbook lets us to proceed to run the playbook with confidence.

### How to verify playbooks in Ansible?

- **Check Mode**: 
  - Ansible's "dry run" where no actual changes are made on hosts.
  - Allows preview of playbook changes without applying them.
  - Use the `--check` option to run a playbook in check mode.

Example:

`install_nginx.yml`
```
---
- hosts: webservers
  tasks:
    - name: Ensure nginx is installed
      apt:
        name: nginx
        state: present
      become: yes
```

Run command:
```
ansible-playbook install-nginx.yml --check
```

- In the output the playbook will say that it will change the web server but as we run it in check mode, it wont do anything.

- **Diff Mode**:
  - Provides a before-and-after comparison of playbook changes.
  - Undestand and verify the impact of playbooks changes before applying them.
  - Utilize the `--diff` option to run a playbook in a diff mode.

Example:

`configure_nginx.yml`
```
- hosts: webservers
  tasks:
    - name: Ensure the configuration line is present
      lineinfile:
        path: /etc/nginx/nginx.conf
        line: 'client_max_body_size 100M;'
      become: yes
```

Run:

```
ansible-playbook configure_nginx.yml --check --diff
```

## Syntax Check

- This mode checks the syntax of your playbook for any errors.

- Ensures playbook is error-free.

- To run a syntax check on a playbook:

`configure_nginx.yml`
```
---
- hosts: webservers
  tasks:
    - name: Ensure the configuration line is present
      lineinfile:
        path: /etc/nginx/nginx.conf
        line: 'client_max_body_size 100M;'
      become: yes
```

```
ansible-playbook configure_nginx.yml --syntax-check
```
