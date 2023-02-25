
```docker
docker exec -it oracle bash
```

```sql
sqlplus /nolog
connect sys as sysdba;
alter session set "_ORACLE_SCRIPT"=true;
create user dhb identified by 12345;
GRANT ALL PRIVILEGES TO dhb;
```