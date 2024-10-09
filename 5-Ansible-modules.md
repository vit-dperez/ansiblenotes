# Ansible Modules

## Ansible modules

Ansible modules are categorized in many groups based on their functionality.

- `System`: This modules are actions to be performed at a system level such as modifying the users and groups on the system, modifying IP tables, firewall configurations on the system, working with logical volume groups, mounting operations or working with services.
  - starting services
  - restarting services

- `Command`: Are used to execute commands, or scripts on the hosts, this could be as simple as using the `command` module or an interactive execution using the `expect` module, by responding to prompts.

- `File`: File modules help work with the files, use the `ACL` module to set and retrieve ACL information on files, use the `archive` and `un-archive` modules to compress and unpack files, use `find`, `lineinfile` and `replace` modules to modify the contents of an existing file.

- `Database`: Help in working with databases, such as `MongoDB`, `MySQL`, `MSSQL` or `PostgreSQL`, to add or remove database or modify database configurations.

- `Cloud`: The cloud section has a vast collection of modules, for various different cloud providers, like:
  - Amazon
  - Cloudstack
  - Azure
  - Docker
  - Digital Ocean
  - Google
  - Rackspace
  - Openstack
  - VMware

- `Windows`: This module helps to use Ansible in a Windows environment, some of this are `wincopy` to copy files, `wincommand` to execute a command on a Windows machine, create a IIS website, install a software using MSI installer, make changes to registry using reg edit, and manage users and services on Windows.

- Get the documentation of modules on: `docs.ansible.com`

### Using modules (Command)

- Executes a command on a remote node

- Params:
  - `chdir`: cd into this directory before running the command.
  - `ceates`: a filename or glob pattern, when it already exists, this step will `not` be run.
  - `executable`: change the shell used to execute the command. Should be an absoulte path to the executable.
  - `free_form`: the command module takes a free form command to run. There is no parameter actually named 'free form'. See the examples!
  - `removes`: a filename or glob pattern, when it does not exist, this step will `not` be run.
  - `warn`: if ac ommand warinings are on in ansible.cfg, do not warn about this particular line if set to no/false.

- Example:

`playbook.yaml`.

```
-
  name: Play `
  hosts: localhost
  tasks:
    - name: Execute command 'date'
      command: date

    - name: Display resolv.conf contents
      command: cat resolv.conf chdir=/etc

    - name: Create the folder "folder"
      command: mkdir /folder creates=/folder
```

### Using modules (script)

- Runs a local script on a remote node after transferring it.

```
-
  name: Play 1
  hosts: localhost
  tasks:
    - name: Run a script on remote server
      script: /some/local/script.sh -arg1 -arg2
```

### Using modules (Service)

- Manage Services - Start, Stop, Restart

`playbook.yml`.
```
-
  name: Start Services in order
  hosts: localhost
  tasks:
    - name: Start the database service
      service: name=postgresql state=started

    - name: Start the httpd service
      service: name=httpd state=started

    - name: Start nginx service
      service: name=nginx state=started
```
```
-
  name:
  hosts: localhost
  tasks:
    - name: Start the database service
      serice:
        name: postgresql
        state: started
```

### Using modules (lineinfile)

- Search for a line in a file and replace it or add it if it doesn't exist.

- For example if you are given the task to add the line `nameserver 10.1.250.10` in the `/etc/resolv.conf` file we use this module.

`/etc/resolv.conf`
```
nameserver 10.1.250.1
nameserver 10.1.250.2
```

`playbook.yml`
```
-
  name: Add DNS server to resolv.conf
  hosts: localhost
  tasks:
    - lineinfile:
        path: /etc/resolv.conf
        line: 'nameserver 10.1.250.10'
```

## Ansible Plugins

- Although Ansible has a lot of built-in modules and functionality we sometimes need to rely on some extra functionality that can dynamically fetch real-time information about cloud resources, such as instances, security groups or tags, directly from the cloud provider's API to ensure the inventory remains up to date and reflects the current state of the infrastructure.

- To address this challenges we can leverage `Ansible plugins`, wich provide extensibility and customization options beyond the core Ansible features.

- A piece of code that modifies the functionality of Ansible.

- Enhance various aspects of Ansible:
  - Inventory
  - Modules
  - Callbacks

- Flexible and Powerful way to customize.

- It can be on the format of:
  - Inventory plugin
  - Module plugin
  - Action plugin
  - Callback plugin