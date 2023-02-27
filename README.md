Use Debezium to send Oracle database data to PostgreSQL database through CDC.


# Preparations
1. Sign in https://container-registry.oracle.com/ , then click Database > Enterprise > Tags, select the database version to use.
2. pull oracle database image
```shell
docker login container-registry.oracle.com
```
after successful login, execute the following command
```shell
docker pull container-registry.oracle.com/database/enterprise:12.2.0.1
```
it may take a few minutes.

3. delete all files under the **oracle>ORCLCDB** folder of this project
4. create empty logminer file
```shell
cd oracle
mkdir recovery_area
cd ORCLCDB
touch logminer_tbs.dbf
mkdir ORCLPDB1
cd ORCLPDB1
touch logminer_tbs.dbf
```

# Step
1. docker-compose up -d
2. configure the Oracle database,Refs [Debezium Oracle Connetor Documentation](https://debezium.io/documentation/reference/2.1/connectors/oracle.html#_preparing_the_database)
**Notes** : before executing the following commands, switch sessions
```sql
ALTER SESSION SET CONTAINER = ORCLPDB1;
-- https://debezium.io/documentation/reference/2.1/connectors/oracle.html#creating-users-for-the-connector
CREATE TABLESPACE logminer_tbs DATAFILE '/opt/oracle/oradata/ORCLCDB/ORCLPDB1/logminer_tbs.dbf' SIZE 25M REUSE AUTOEXTEND ON MAXSIZE UNLIMITED;
```
3. create connectors in kafka-connect
```shell
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @oracle-source.json
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @postgre-sink.json
```

# Verification
1. check if data is synchronized to PostgreSQL
