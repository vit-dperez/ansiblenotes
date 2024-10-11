### Ansible Handlers

- It is a task that is run when certain file.

- Ex: when there is an update on the code and it is modified in a webserver the server will automatically restart when the file is modified.

```
- name: Deploy Application
  hosts: application_servers
  tasks:
    - name: Copy Application Code
      copy:
        src: app_code/
        dest: /opt/application/
      notify: Restart Application Service

  handlers:
    - name: Restart Application Service
      service:
        name: application_service
        state: restarted
```

### Ansible Roles

- Assigning a role means doing everything you need to do to make a server a mysql server or webserver.

`playbook.yml`
```
- name: Install and Configure Mysql
  hosts: db-server1
  roles:
    - mysql
```

`mysql-role.yml`
```
tasks:
  - name: Install Pre-Requisites
    yum: name=pre-req-packages state=present

  - name: Install MySQL Packages
    yum: name=mysql state=present

  - name: Start MySQL Service
    service: name=mysql state=started

  - name: Configure Database
    mysql_db: name=db1 state=present
```

- Ansible galaxy command will create the directory structure for a role

```
ansible-galaxy init mysql
```

**Output**:
- mysql/
  - README.md
  - templates/
  - tasks/
  - handlers/
  - vars/
  - defaults/
  - meta/

- The roles can be specified in a directory inside the playbook's directory or in `/etc/ansible/roles` which is the default location where ansible will look for roles.

- Also the roles are specified in `roles_path` variable.

- You can install roles previously created with the `ansible-galaxy` command.

```
ansible-galaxy list
```

### What Are Ansible Collections?

- Package and distribute modules, roles, plugins, etc.