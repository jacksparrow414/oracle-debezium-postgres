
```docker
docker exec -it oracle bash
```

```sql
sqlplus sys/12345@//localhost:1521/ORCLCDB.localdomain as SYSDBA
username: sys as sysdba
```
```sql
sqlplus /nolog
connect sys as sysdba;
alter session set "_ORACLE_SCRIPT"=true;
create user dhb identified by 12345;
GRANT ALL PRIVILEGES TO dhb;
```

```sql
CREATE TABLESPACE logminer_tbs DATAFILE '/opt/oracle/oradata/ORCLCDB/logminer_tbs.dbf' SIZE 25M REUSE AUTOEXTEND ON MAXSIZE UNLIMITED;
```

```sql
CONN / AS SYSDBA
ALTER SESSION SET CONTAINER = ORCLPDB1;
SHOW CON_NAME
CREATE TABLESPACE logminer_tbs DATAFILE '/opt/oracle/oradata/ORCLCDB/ORCLPDB1/logminer_tbs.dbf' SIZE 25M REUSE AUTOEXTEND ON MAXSIZE UNLIMITED;
```

```sql
sqlplus sys/12345@//localhost:1521/ORCLPDB1.localdomain as sysdba
alter session set "_ORACLE_SCRIPT"=true;
CREATE USER c##dbzuser IDENTIFIED BY dbz DEFAULT TABLESPACE logminer_tbs QUOTA UNLIMITED ON logminer_tbs CONTAINER=ALL;
```