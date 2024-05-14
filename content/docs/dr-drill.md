---
title: Switchover to Physical Standby
type: docs
prev: /
next: docs/folder/
---

DR-Drill steps

Disable Export Backup
Sync check


on PRODUCTION database --

```
select name, open_mode, database_role, switchover_status from v$database;
```

NAME      OPEN_MODE            DATABASE_ROLE    SWITCHOVER_STATUS
--------- -------------------- 	---------------- --------------------
SFMS      READ WRITE           PRIMARY          TO STANDBY


on STANDBY server --
```
select name, open_mode, database_role, switchover_status from v$database;
```
NAME      OPEN_MODE            DATABASE_ROLE    SWITCHOVER_STATUS
--------- -------------------- 	---------------- --------------------
SFMS      READ ONLY           PHYSICAL STANDBY          	NOT ALLOWED


## on PRODUCTION Server

```
switch logfile x 2
```
```
alter system switch logfile;
```
```
archive log list
```
```
alter database commit to switchover to physical standby with session shutdown;
```

### Exit from SQL Prompt

database altered.23:15 23-09-202223:15 23-09-202223:15 23-09-202223:15 23-09-202223:15 23-09-202223:15 23-09-202223:15 23-09-2022


on STANDBY server --

SQL> select name, open_mode, database_role, switchover_status from v$database;

NAME      OPEN_MODE            DATABASE_ROLE    SWITCHOVER_STATUS
--------- -------------------- 	---------------- --------------------
SFMS      READ ONLY           PHYSICAL STANDBY   TO PRIMARY


SQL> alter database commit to switchover to primary;

database altered.


SQL> alter database open;

database altered.

select process, status, sequence# from v$managed_satandby;

### Start Recovery Process
```
select process, status, sequence# from v$managed_standby;

alter database recover managed standby database disconnect from session;

select max(sequence#) from v$archived_log where applied ='YES';
```
