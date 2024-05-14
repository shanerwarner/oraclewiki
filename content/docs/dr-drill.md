---
title: Switchover to Physical Standby
type: docs
prev: /
next: docs/folder/
---


## on PRODUCTION database

```
select name, open_mode, database_role, switchover_status from v$database;
```

NAME      OPEN_MODE            DATABASE_ROLE    SWITCHOVER_STATUS
--------- -------------------- 	---------------- --------------------
SFMS      READ WRITE           PRIMARY          TO STANDBY


## on STANDBY Database
```
select name, open_mode, database_role, switchover_status from v$database;
```
NAME      OPEN_MODE            DATABASE_ROLE    SWITCHOVER_STATUS
--------- -------------------- 	---------------- --------------------
SFMS      READ ONLY           PHYSICAL STANDBY          	NOT ALLOWED


# Proceed to Switchover

### on PRODUCTION Database


#### switch the logfile x 2 times

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

Output: **database altered.**


### on STANDBY Database
```
select name, open_mode, database_role, switchover_status from v$database;
```
NAME      OPEN_MODE            DATABASE_ROLE    SWITCHOVER_STATUS
--------- -------------------- 	---------------- --------------------
SFMS      READ ONLY           PHYSICAL STANDBY   TO PRIMARY

```
alter database commit to switchover to primary;
```

Output: **database altered.**

```
alter database open;
```

Output: **database altered.**

```
select process, status, sequence# from v$managed_satandby;
```

### Start Recovery - MRP Process
```
select process, status, sequence# from v$managed_standby;

alter database recover managed standby database disconnect from session;

select max(sequence#) from v$archived_log where applied ='YES';
```


END of Document