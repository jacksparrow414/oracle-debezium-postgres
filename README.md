
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

```sql
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @oracle-source.json

curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @postgre-sink.json

curl -X DELETE http://localhost:8083/connectors/jdbc-sink-customers-postgress

kafka-topics.sh --bootstrap-server kafka1:9022,kafka2:9022,kafka3:9022 --delete --topic __consumer_offsets

kafka-console-consumer.sh --from-beginning --bootstrap-server kafka1:9092 --topic server1.C__DBZUSER.CONTACT
```