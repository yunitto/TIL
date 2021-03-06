# Managed and External Tables

- [cwiki.apache.org](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-CreateTableCreate/Drop/TruncateTable
)
>By default Hive creates managed tables, where files, metadata and statistics are managed by internal Hive processes. A managed table is stored under the hive.metastore.warehouse.dir path property, by default in a folder path similar to /apps/hive/warehouse/databasename.db/tablename/. The default location can be overridden by the location property during table creation. If a managed table or partition is dropped, the data and metadata associated with that table or partition are deleted. If the PURGE option is not specified, the data is moved to a trash folder for a defined duration.
<br><br>Use managed tables when Hive should manage the lifecycle of the table, or when generating temporary tables.
<br><br>An external table describes the metadata / schema on external files. External table files can be accessed and managed by processes outside of Hive. External tables can access data stored in sources such as Azure Storage Volumes (ASV) or remote HDFS locations. If the structure or partitioning of an external table is changed, an MSCK REPAIR TABLE table_name statement can be used to refresh metadata information.
<br><br>Use external tables when files are already present or in remote locations, and the files should remain even if the table is dropped.
<br><br>Managed or external tables can be identified using the DESCRIBE FORMATTED table_name command, which will display either MANAGED_TABLE or EXTERNAL_TABLE depending on table type.

- [Stack Overflow: Difference between Hive internal tables and external tables?](https://stackoverflow.com/questions/17038414/difference-between-hive-internal-tables-and-external-tables)
- [What is the difference between CREATE TABLE AND CREATE EXTERNAL TABLE ?](http://data-flair.training/forums/topic/what-is-the-difference-between-create-table-and-create-external-table)

# How to modify data & schema of HIVE managed table
## Problems
- Managed table `db.my_tbl` is created with 'location' clause.
- Need to add new columns, not only for upcoming data but also for previous data.
- Need to keep backup of data, in case of failures.
- HDFS Location of `db.my_tbl` data should not change.
- `insert overwrite ...` takes too much time.

## Notes
- `alter table db.my_tbl set location 'hdfs://path/to/new_data'` does not change the actual location of data. It only modifies the metadata about where upcoming data will be stored, not about current data. So even if I modify location metadata to `'path/to/new_data'` and move the hdfs data to `'path/to/new_data'`, Hive cannot access the data. This is because the Hive manages internal tables.
- ```The CASCADE|RESTRICT clause is available in Hive 1.1.0. ALTER TABLE CHANGE COLUMN with CASCADE command changes the columns of a table's metadata, and cascades the same change to all the partition metadata. RESTRICT is the default, limiting column change only to table metadata.```

## Solution
1. Replace columns metadata
2. ~ 3. Swap the AS-IS and TO-BE data.
```
1. alter table db.my_tbl replace columns (..) cascade
2. hadoop fs -mv 'hdfs://path/to/data' 'hdfs://path/to/data_backup'
3. hadoop fs -mv 'hdfs://path/to/new_data' 'hdfs://path/to/data'
```
