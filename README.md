# How to migrate from Galera Cluster to MySQL Group Replication
```
http://lefred.be/content/how-to-migrate-from-galera-cluster-to-mysql-group-replication/

 ansible-playbook /vagrant/ansible/mysql.yml  -i /vagrant/ansible/hosts/all

 service mysqld start
  service mysqld status

```

A superuser account 'root'@'localhost is created. A password for the superuser is set and stored in the error log file. To reveal it, use the following command:
```
shell> sudo grep 'temporary password' /var/log/mysqld.log
```

Change the root password as soon as possible by logging in with the generated, temporary password and set a custom password for the superuser account:

```
shell> mysql -uroot -p
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```