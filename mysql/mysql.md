# MYSQL

## Check server status

```bash
sudo service mysql status
```

If the server is not running correctly, you can type the following command to start it:

```bash
sudo service mysql restart
```

## Configure MySQL

You can edit the files in `/etc/mysql/` to configure the basic settings – log file, port number, etc. For example, to configure MySQL to listen for connections from network hosts, in the file `/etc/mysql/mysql.conf.d/mysqld.cnf`, change the bind-address directive to the server’s IP address:

    bind-address            = 192.168.0.5

> **NOTE:**
> Replace 192.168.0.5 with the appropriate address, which can be determined via the ip address show command.

After making a configuration change, the MySQL daemon will need to be restarted with the following command:

```bash
sudo systemctl restart mysql.service
```

## Advanced configuration

### Creating a tuned configuration

There are a number of parameters that can be adjusted within MySQL’s configuration files. This will allow you to improve the server’s performance over time.

- First, if you have existing data, you will first need to carry out a **mysqldump** and reload:

    ```bash
    mysqldump --all-databases --routines -u root -p > ~/fulldump.sql
    ```

- Once the dump has been completed, shut down MySQL:

    ```bash
    sudo service mysql stop
    ```

- It’s also a good idea to backup the original configuration:

    ```bash
    sudo rsync -avz /etc/mysql /root/mysql-backup
    ```

- Next, **make any desired configuration changes**. Then, delete and re-initialise the database space and make sure ownership is correct before restarting MySQL:

    ```bash
    sudo rm -rf /var/lib/mysql/*
    sudo mysqld --initialize
    sudo chown -R mysql: /var/lib/mysql
    sudo service mysql start
    ```

- The final step is re-importation of your data by piping your SQL commands to the database.

    ```bash
    cat ~/fulldump.sql | mysql
    ```

For large data imports, the ‘Pipe Viewer’ utility can be useful to track import progress. Ignore any ETA times produced by pv; they’re based on the average time taken to handle each row of the file, but the speed of inserting can vary wildly from row to row with mysqldumps:

```bash
sudo apt install pv
pv ~/fulldump.sql | mysql
```

*Once this step is complete, you are good to go!*

> **NOTE:**
> This is not necessary for all my.cnf changes. Most of the variables you can change to improve performance are adjustable even whilst the server is running. As with anything, make sure to have a good backup copy of your config files and data before making changes.

## Change user authentication

Then run the following `ALTER USER` command to change the root user’s authentication method to one that uses a password. The following example changes the authentication method to `mysql_native_password`:

```sql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

## The mysql_secure_installation command
is a script provided by MySQL to help improve the security of a MySQL installation. When you run this command, it guides you through a series of steps to configure various security settings for your MySQL server. Here’s a brief overview of what it does:

1. **Set the Root Password**: It allows you to set or change the password for the MySQL root user if it hasn't been set yet.

2. **Remove Anonymous Users**: It can remove any anonymous users that may be created by default, which is a security risk as they can connect to the database without a password.

3. **Disallow Root Login Remotely**: It can restrict the root user’s ability to log in from remote machines, limiting access to only the local host. This is a security measure to reduce exposure to potential attacks.

4. **Remove the Test Database:** It removes the default test database that comes pre-installed, which is generally not needed in a production environment and could be a security risk if left accessible.

5. **Reload Privilege Tables**: It reloads the privilege tables to ensure that all changes made during the process are applied immediately.

```bash
sudo mysql_secure_installation
```

## Creating MySQL User and Granting Privileges

```sql
mysql> CREATE USER 'username'@'host' IDENTIFIED WITH authentication_plugin BY 'password';
```

[The MySQL documentation recommends](https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html#upgrade-caching-sha2-password) this plugin for users

After creating your new user, you can grant them the appropriate privileges. The general syntax for granting user privileges is as follows:

```sql
mysql> GRANT PRIVILEGES ON database.table TO 'username'@'host';
```

You can grant multiple privileges to the same user in one command by separating each with a comma. You can also grant a user privileges globally by entering asterisks (*) in place of the database and table names. In SQL, asterisks are special characters used to represent “all” databases or tables.

```sql
mysql> GRANT CREATE, ALTER, DROP, INSERT, UPDATE, INDEX, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'dev'@'localhost' WITH GRANT OPTION;
```

Following this, it’s good practice to run the `FLUSH PRIVILEGES` command. This will free up any memory that the server cached as a result of the preceding CREATE USER and GRANT statements:

```sql
mysql> FLUSH PRIVILEGES;
```
> **NOTE:**
> this statement also includes `WITH GRANT OPTION`. This will allow your MySQL user to grant any permissions that it has to other users on the system.

You can find the full list of available privileges [in the official MySQL documentation.](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#privileges-provided-summary)

## Better performance with MySQL Tuner

MySQL Tuner is a Perl script that connects to a running MySQL instance and offers configuration suggestions for optimising the database for your workload. The longer the server has been running, the better the advice mysqltuner can provide. In a production environment, consider waiting for at least 24 hours before running the tool. You can install mysqltuner with the following command:

```bash
sudo apt install mysqltuner
```

Then once it has been installed, simply run: `mysqltuner` – and wait for its final report.
