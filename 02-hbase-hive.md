## Connect HBase and Hive


## Create tables and insert data
- You can use SSH to connect to HBase clusters and then use Apache HBase Shell to create HBase tables, insert data, and query data.

```
ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
```

```
hbase shell
```

```
create 'Contacts', 'Personal', 'Office'
```

```
list
```

```
put 'Contacts', '1000', 'Personal:Name', 'John Dole'
put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
```

```
scan 'Contacts'
```

```
get 'Contacts', '1000'
```

```
exit
```


## Bulk load data into the contacts HBase table
- From your open ssh connection, run the following command to transform the data file to StoreFiles and store at a relative path specified by Dimporttsv.bulk.output.

```
hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
```

- Open HBase shell and check content
```
hbase shell
```

```
scan 'Contacts'
```


- Run the following command to upload the data from /example/data/storeDataFileOutput to the HBase table:- 
```
hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
```

## Use Apache Hive to query Apache HBase
- From your open ssh connection, use the following command to start Beeline:
```
beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
```

- Run the following HiveQL script to create a Hive table that maps to the HBase table. Make sure that you've created the sample table referenced earlier in this article by using the HBase shell before you run this statement.
```
CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
TBLPROPERTIES ('hbase.table.name' = 'Contacts');
```

- Run the following HiveQL script to query the data in the HBase table:
```
SELECT * FROM hbasecontacts;
```
