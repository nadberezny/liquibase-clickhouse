# liquibase-clickhouse
                       
Supported operations: update, rollback (with provided SQL script), tag


Maven dependency:

```
<dependency>
    <groupId>io.github.nadberezny</groupId>
    <artifactId>liquibase-clickhouse</artifactId>
    <version>Latest version from Maven Central</version>
</dependency>
```

The cluster mode can be activated by adding the **_liquibaseClickhouse.conf_** file to the classpath (liquibase/lib/).
```
cluster {
    clusterName="{cluster}"
    tableZooKeeperPathPrefix="/clickhouse/tables/{shard}/{database}/"
    tableReplicaName="{replica}"
}
```
In this mode, liquibase will create its own tables as replicated.<br/>
All changes in these files will be replicated on the entire cluster.<br/>
Your updates should also affect the entire cluster either by using ON CLUSTER clause, or by using replicated tables.

<hr/>

###### Important changes
From the version 0.7.0 the liquibase-clickhouse supports replication on a cluster. Liquibase v4.6.1.

From the version 0.6.0 the extension adapted for the liquibase v4.3.5.

