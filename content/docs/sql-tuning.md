---
title: SQL Tuning
type: docs
prev: /
next: docs/folder/
---


### 1. To check for waiting event in v$session:
```sql
SELECT SID,SERIAL#,STATUS,EVENT,INST_ID FROM GV$SESSION WHERE WAIT_CLASS<>'Idle';
```


### 2. To check SQL id which is waiting for resources:
```sql
SELECT SID,SERIAL#,STATUS,EVENT,INST_ID ,SQL_ID, ROW_WAIT_OBj# FROM GV$SESSION WHERE WAIT_CLASS<>'Idle';
```


### 3. Check when the execution plan of that perticular SQL ID(found in previous session) got changed:
```sql
set lines 255 pages 1000
col execs for 999,999,999
col avg_etime for 999,999.999
col avg_lio for 999,999,999.9
col begin_interval_time for a25
col node for 99999
col PLAN_HASH_VALUE for 999999999999999
column MODULE for a18 trunc
break on plan_hash_value on startup_time skip 1
select ss.snap_id, ss.instance_number inst_id, begin_interval_time, sql_id, plan_hash_value,
nvl(executions_delta,0) execs,
(elapsed_time_delta/decode(nvl(executions_delta,0),0,1,executions_delta))/1000000 avg_etime,
(buffer_gets_delta/decode(nvl(executions_delta,0),0,1,executions_delta)) avg_lio, module,
rows_processed_delta rws
from DBA_HIST_SQLSTAT S, DBA_HIST_SNAPSHOT SS
where sql_id = nvl('&sql_id','4dqs2k5tynk61')
and ss.snap_id = S.snap_id
and ss.instance_number = S.instance_number
and executions_delta > 0
order by 1;
```
Enter The SQL ID when Prompted.


### 4. Check the explain plan for the query:
```sql
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY_AWR('&SQL_ID',FORMAT=>'ADVANCED'));
```


### 5. Run sqltrpt (SQL Tuning report):
```
@E:\app\oracle\product\19.3.0\dbhome\rdbms\admin\sqltrpt.sql
```


### 6. Run following command to find recommendation from oracle:
```sql
select dbms_sqltune.report_tuning_task(:task_name) from dual where :err <> 1;
```


### 7. Check for estimated benefit in findings from oracle and execute the command which was given in findings:
```sql
execute dbms_sqltune.accept_sql_profile(task_name => 'TASK_6137',task_owner => 'SYS', replace => TRUE);
```


### 8. To check why the execution plan got changed:
```sql
select * from v$sql_shared_cursor where sql_id='42hzrp8y7c22g';
```








