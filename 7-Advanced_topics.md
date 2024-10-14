# Advanced Topics

## Introduction to Templating

### What is Templating?

- It is a way for configuring a service for different servers with different values using variables.

- An analogy would be a CEO from a company wanting to send a letter to all the employees but not all the employees have the same name so he creates a template letter and only changes the values for each employee.

- Example template configuring `mysql`

`Template`
```
[mysqld]
innodb-buffer-pool-size={{ pool_size }}
datadir={{ datadir }}
user={{ mysql_user }}
symbolic-links={{ link_id }}
port={{ mysql_port }}
```

`Variables`
```
pool_size: 5242880
datadir: /var/lib/mysql
mysql_user: mysql
link_id: 0
mysql_port: 3306
```

`Outcome`
```
[mysqld]
innodb-buffer-pool-size=5242880
datadir=/var/lib/mysql
user=mysql
symbolic-links=0
port=3306
```

