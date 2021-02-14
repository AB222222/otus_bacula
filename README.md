# otus_bacula

## Установка и настройка как-то так:

me@me-HP-260-G3-DM:~/otus-linux/DZ_OTUS_BACULA$ vagrant ssh baculaserver
[vagrant@baculaserver ~]$ sudo -i
[root@baculaserver ~]# alternatives --config libbaccats.so

There are 3 programs which provide 'libbaccats.so'.

  Selection    Command
-----------------------------------------------
   1           /usr/lib64/libbaccats-mysql.so
   2           /usr/lib64/libbaccats-sqlite3.so
*+ 3           /usr/lib64/libbaccats-postgresql.so

Enter to keep the current selection[+], or type selection number: 1
[root@baculaserver ~]# mysql -u root
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 2
Server version: 5.5.68-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create database bacula;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> grant all on bacula.* to bacula@localhost identified by 'bacula';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> quit;
Bye
[root@baculaserver ~]# /usr/libexec/bacula/make_mysql_tables
Creation of Bacula MySQL tables succeeded.
[root@baculaserver ~]# 

## Файлы клиента и сервера, которые надо положить в /etc/bacula - в директория server и client

## Джобы:

*list jobs
+-------+-------------------------------------------+---------------------+------+-------+----------+------------+-----------+
| JobId | Name                                      | StartTime           | Type | Level | JobFiles | JobBytes   | JobStatus |
+-------+-------------------------------------------+---------------------+------+-------+----------+------------+-----------+
|     1 | Backup configs incremental server 1       | 2021-02-13 19:51:02 | B    | F     |        0 |          0 | f         |
|     2 | Backup configs incremental server 1       | 2021-02-13 20:01:02 | B    | F     |        0 |          0 | f         |
|     3 | Backup configs differential server 1      | 2021-02-13 20:05:03 | B    | F     |    2,431 | 27,919,142 | T         |
|     4 | Backup configs incremental 192.168.10.23  | 2021-02-14 06:31:15 | B    | F     |    2,431 | 27,919,142 | T         |
|     5 | Backup configs differential 192.168.10.23 | 2021-02-14 06:35:02 | B    | F     |    2,431 | 27,919,142 | T         |
+-------+-------------------------------------------+---------------------+------+-------+----------+------------+-----------+
*run
A job name must be specified.
The defined Job resources are:
     1: Backup configs incremental 192.168.10.23
     2: Backup configs differential 192.168.10.23
     3: Backup configs full 192.168.10.23
Select Job resource (1-3): 3
Run Backup job
JobName:  Backup configs full 192.168.10.23
Level:    Incremental
Client:   client1-fd
FileSet:  Linux Configs
Pool:     File (From Job resource)
Storage:  File (From Job resource)
When:     2021-02-14 06:38:14
Priority: 10
OK to run? (yes/mod/no): y
Job queued. JobId=6
*


*list files jobid=6
.....
| /etc/audit/auditd.conf |
| /etc/audit/rules.d/audit.rules |
| /etc/audit/audit.rules |
| /etc/qemu-ga/fsfreeze-hook |
| /etc/chrony.conf |
| /etc/chrony.keys |
| /etc/rsyncd.conf |
| /etc/rsyslog.conf |
| /etc/man_db.conf |
| /etc/sudo-ldap.conf |
| /etc/sudo.conf |
| /etc/sudoers |
| /etc/sudoers.d/vagrant |
| /etc/e2fsck.conf |
| /etc/mke2fs.conf |
| /etc/vconsole.conf |
| /etc/locale.conf |
| /etc/hostname |
| /etc/.updated |
| /etc/aliases.db |
| /etc/nanorc |
| /etc/ntp.conf |
| /etc/bacula/bacula-fd.conf |
+----------+
+-------+-----------------------------------+---------------------+------+-------+----------+------------+-----------+
| JobId | Name                              | StartTime           | Type | Level | JobFiles | JobBytes   | JobStatus |
+-------+-----------------------------------+---------------------+------+-------+----------+------------+-----------+
|     6 | Backup configs full 192.168.10.23 | 2021-02-14 06:38:20 | B    | F     |    2,431 | 27,919,142 | T         |
+-------+-----------------------------------+---------------------+------+-------+----------+------------+-----------+
*list jobs
+-------+-------------------------------------------+---------------------+------+-------+----------+------------+-----------+
| JobId | Name                                      | StartTime           | Type | Level | JobFiles | JobBytes   | JobStatus |
+-------+-------------------------------------------+---------------------+------+-------+----------+------------+-----------+
|     1 | Backup configs incremental server 1       | 2021-02-13 19:51:02 | B    | F     |        0 |          0 | f         |
|     2 | Backup configs incremental server 1       | 2021-02-13 20:01:02 | B    | F     |        0 |          0 | f         |
|     3 | Backup configs differential server 1      | 2021-02-13 20:05:03 | B    | F     |    2,431 | 27,919,142 | T         |
|     4 | Backup configs incremental 192.168.10.23  | 2021-02-14 06:31:15 | B    | F     |    2,431 | 27,919,142 | T         |
|     5 | Backup configs differential 192.168.10.23 | 2021-02-14 06:35:02 | B    | F     |    2,431 | 27,919,142 | T         |
|     6 | Backup configs full 192.168.10.23         | 2021-02-14 06:38:20 | B    | F     |    2,431 | 27,919,142 | T         |
|     7 | Backup configs incremental 192.168.10.23  | 2021-02-14 06:41:02 | B    | I     |        5 |        714 | T         |
|     8 | Backup configs incremental 192.168.10.23  | 2021-02-14 06:51:02 | B    | I     |        0 |          0 | T         |
|     9 | Backup configs incremental 192.168.10.23  | 2021-02-14 07:01:02 | B    | I     |        0 |          0 | T         |
|    10 | Backup configs differential 192.168.10.23 | 2021-02-14 07:05:02 | B    | D     |        0 |          0 | T         |
|    11 | Backup configs incremental 192.168.10.23  | 2021-02-14 07:11:02 | B    | I     |        0 |          0 | T         |
|    12 | Backup configs incremental 192.168.10.23  | 2021-02-14 07:21:02 | B    | I     |        0 |          0 | T         |
|    13 | Backup configs incremental 192.168.10.23  | 2021-02-14 07:31:02 | B    | I     |        0 |          0 | T         |
|    14 | Backup configs differential 192.168.10.23 | 2021-02-14 07:35:02 | B    | D     |        0 |          0 | T         |
|    15 | Backup configs incremental 192.168.10.23  | 2021-02-14 07:41:02 | B    | I     |        0 |          0 | T         |
|    16 | Backup configs incremental 192.168.10.23  | 2021-02-14 07:51:02 | B    | I     |        0 |          0 | T         |
|    17 | Backup configs incremental 192.168.10.23  | 2021-02-14 08:01:02 | B    | I     |        0 |          0 | T         |
|    18 | Backup configs differential 192.168.10.23 | 2021-02-14 08:05:02 | B    | D     |        0 |          0 | T         |
|    19 | Backup configs incremental 192.168.10.23  | 2021-02-14 08:11:02 | B    | I     |        0 |          0 | T         |
|    20 | Backup configs incremental 192.168.10.23  | 2021-02-14 08:21:02 | B    | I     |        0 |          0 | T         |
|    21 | Backup configs incremental 192.168.10.23  | 2021-02-14 08:31:02 | B    | I     |        0 |          0 | T         |
|    22 | Backup configs differential 192.168.10.23 | 2021-02-14 08:35:03 | B    | D     |        0 |          0 | T         |
|    23 | Backup configs incremental 192.168.10.23  | 2021-02-14 08:41:03 | B    | I     |        0 |          0 | T         |
|    24 | Backup configs incremental 192.168.10.23  | 2021-02-14 08:51:03 | B    | I     |        0 |          0 | T         |
|    25 | Backup configs incremental 192.168.10.23  | 2021-02-14 09:01:03 | B    | I     |        0 |          0 | T         |
|    26 | Backup configs differential 192.168.10.23 | 2021-02-14 09:05:03 | B    | D     |        0 |          0 | T         |
|    27 | Backup configs incremental 192.168.10.23  | 2021-02-14 09:11:03 | B    | I     |        0 |          0 | T         |
|    28 | Backup configs incremental 192.168.10.23  | 2021-02-14 09:21:03 | B    | I     |        0 |          0 | T         |
|    29 | Backup configs incremental 192.168.10.23  | 2021-02-14 09:31:03 | B    | I     |        0 |          0 | T         |
|    30 | Backup configs differential 192.168.10.23 | 2021-02-14 09:35:03 | B    | D     |        0 |          0 | T         |
+-------+-------------------------------------------+---------------------+------+-------+----------+------------+-----------+
*

