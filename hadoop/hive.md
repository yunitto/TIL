# Managed and External Tables

- [cwiki.apache.org](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-CreateTableCreate/Drop/TruncateTable
)
>By default Hive creates managed tables, where files, metadata and statistics are managed by internal Hive processes. A managed table is stored under the hive.metastore.warehouse.dir path property, by default in a folder path similar to /apps/hive/warehouse/databasename.db/tablename/. The default location can be overridden by the location property during table creation. If a managed table or partition is dropped, the data and metadata associated with that table or partition are deleted. If the PURGE option is not specified, the data is moved to a trash folder for a defined duration.
<br><br>Use managed tables when Hive should manage the lifecycle of the table, or when generating temporary tables.
<br><br>An external table describes the metadata / schema on external files. External table files can be accessed and managed by processes outside of Hive. External tables can access data stored in sources such as Azure Storage Volumes (ASV) or remote HDFS locations. If the structure or partitioning of an external table is changed, an MSCK REPAIR TABLE table_name statement can be used to refresh metadata information.
<br><br>Use external tables when files are already present or in remote locations, and the files should remain even if the table is dropped.
<br><br>Managed or external tables can be identified using the DESCRIBE FORMATTED table_name command, which will display either MANAGED_TABLE or EXTERNAL_TABLE depending on table type.

- [Stack Overflow: Difference between Hive internal tables and external tables?](https://stackoverflow.com/questions/17038414/difference-between-hive-internal-tables-and-external-tables)
