# How to migrate from Galera Cluster to MySQL Group Replication
```
http://lefred.be/content/how-to-migrate-from-galera-cluster-to-mysql-group-replication/

 ansible-playbook /vagrant/ansible/mysql.yml  -i /vagrant/ansible/hosts/all

```
## Starting MySQL
```
$ service mysqld start
$ sudo systemctl start mysqld
$ service mysqld status
$ sudo systemctl status mysqld
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

## Configuring MySQL
https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-centos-7

## Testing MySQL
```
mysqladmin -u root -p version
```
# ProxySQL Tutorial : setup in a MySQL replication topology
For this tutorial, we will use a small setup with 4 nodes:
- mysql-01 (10.100.194.201) , mysql master
- mysql-02 (10.100.194.202) , mysql slave
- mysql-03 (10.100.194.203) , mysql slave 
- app (110.100.194.204) , application server where proxysql will also run

### Replication setup

Without going into details on how to configure mysqld in the nodes (this is outside the scope of this tutorial), the nodes have already mysqld up and running, configure to support gtid, but replication is not running yet. We will configure replication during this tutorial from "app".

### Create root user user,  replication user, setup replication, and create application user

First, let's create the root user to connect from app:
```
vagrant@app:~$ for i in 1 2 3 ; do
ssh mysql-0$i -- 'mysql -uroot -proot -e "GRANT ALL PRIVILEGES ON *.* TO root@app IDENTIFIED BY \"MyNewPass4!\" WITH GRANT OPTION" -NB'
done
Warning: Using a password on the command line interface can be insecure.
Warning: Using a password on the command line interface can be insecure.
Warning: Using a password on the command line interface can be insecure.
```

