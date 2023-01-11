# Explicando o desenvolvimento do Script "SQOOP" :pencil:

Esse foi o script onde mostro um pouco do desenvolvimento das transações de dados com o SQOOP.

<br>

> Podemos identificar como os comandos que inseri aquelas instruções que estão a frente do indicador "[cloudera@quickstart ~]$".

<br>

Abaixo, no prompt do linux, nós listamos os databases presentes e nos conectamos ao **mysql**.

```
[cloudera@quickstart ~]$ sqoop list-databases --connect jdbc:mysql://localhost/ --username root --password cloudera
Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
22/12/22 12:26:32 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
22/12/22 12:26:32 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
22/12/22 12:26:33 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
information_schema
cm
firehose
hue
metastore
mysql
nav
navms
oozie
retail_db
rman
sentry
```

<br>

E, ainda no prompt do linux, nós listamos as tabelas presentes no database, que nós nos conectamos, **mysql**.

```
[cloudera@quickstart ~]$ sqoop list-tables --connect jdbc:mysql://localhost/retail_db --username root --password cloudera
Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
22/12/22 12:27:36 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
22/12/22 12:27:36 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
22/12/22 12:27:37 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
categories
customers
departments
order_items
orders
products
```

<br>

E, ainda no shell do linux, nós importamos todas as tabelas para retail_db, e fizemos isso nos conectando ao servidor e banco de dados, sinalizamos que é uma importação para o hive, e sobrescrevemos o database que criamos "retail_db".

```
[cloudera@quickstart ~]$ sqoop import-all-tables --connect jdbc:mysql://localhost/retail_db --username root --password cloudera --hive-import --hive-overwrite --hive-database retail_db --create-hive-table --m 1;
Warning: /usr/lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
22/12/22 12:31:32 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.12.0
22/12/22 12:31:32 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
22/12/22 12:31:32 INFO tool.BaseSqoopTool: Using Hive-specific delimiters for output. You can override
22/12/22 12:31:32 INFO tool.BaseSqoopTool: delimiters with --fields-terminated-by, etc.
22/12/22 12:31:32 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
22/12/22 12:31:34 INFO tool.CodeGenTool: Beginning code generation
22/12/22 12:31:34 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `categories` AS t LIMIT 1
22/12/22 12:31:34 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `categories` AS t LIMIT 1
22/12/22 12:31:34 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-cloudera/compile/a4de06e7be51786e8e8e3c227022cf2c/categories.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
22/12/22 12:31:50 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-cloudera/compile/a4de06e7be51786e8e8e3c227022cf2c/categories.jar
22/12/22 12:31:51 WARN manager.MySQLManager: It looks like you are importing from mysql.
22/12/22 12:31:51 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
22/12/22 12:31:51 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
22/12/22 12:31:51 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
22/12/22 12:31:51 INFO mapreduce.ImportJobBase: Beginning import of categories
22/12/22 12:31:51 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
22/12/22 12:31:53 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
22/12/22 12:31:56 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
22/12/22 12:31:56 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
22/12/22 12:32:10 INFO db.DBInputFormat: Using read commited transaction isolation
22/12/22 12:32:10 INFO mapreduce.JobSubmitter: number of splits:1
22/12/22 12:32:10 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1671629034454_0014
22/12/22 12:32:12 INFO impl.YarnClientImpl: Submitted application application_1671629034454_0014
22/12/22 12:32:12 INFO mapreduce.Job: The url to track the job: http://quickstart.cloudera:8088/proxy/application_1671629034454_0014/
22/12/22 12:32:12 INFO mapreduce.Job: Running job: job_1671629034454_0014
22/12/22 12:32:36 INFO mapreduce.Job: Job job_1671629034454_0014 running in uber mode : false
22/12/22 12:32:36 INFO mapreduce.Job:  map 0% reduce 0%
22/12/22 12:32:52 INFO mapreduce.Job:  map 100% reduce 0%
22/12/22 12:32:55 INFO mapreduce.Job: Job job_1671629034454_0014 completed successfully
22/12/22 12:32:55 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=151453
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=87
		HDFS: Number of bytes written=1029
		HDFS: Number of read operations=4
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=2
	Job Counters 
		Launched map tasks=1
		Other local map tasks=1
		Total time spent by all maps in occupied slots (ms)=13220
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=13220
		Total vcore-milliseconds taken by all map tasks=13220
		Total megabyte-milliseconds taken by all map tasks=13537280
	Map-Reduce Framework
		Map input records=58
		Map output records=58
		Input split bytes=87
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=139
		CPU time spent (ms)=990
		Physical memory (bytes) snapshot=176316416
		Virtual memory (bytes) snapshot=1557803008
		Total committed heap usage (bytes)=174587904
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=1029
22/12/22 12:32:55 INFO mapreduce.ImportJobBase: Transferred 1.0049 KB in 59.3107 seconds (17.3493 bytes/sec)
22/12/22 12:32:55 INFO mapreduce.ImportJobBase: Retrieved 58 records.
22/12/22 12:32:55 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `categories` AS t LIMIT 1
22/12/22 12:32:55 INFO hive.HiveImport: Loading uploaded data into Hive

Logging initialized using configuration in jar:file:/usr/lib/hive/lib/hive-common-1.1.0-cdh5.12.0.jar!/hive-log4j.properties
OK
Time taken: 6.053 seconds
Loading data to table retail_db.categories
chgrp: changing ownership of 'hdfs://quickstart.cloudera:8020/user/hive/warehouse/retail_db.db/categories': User does not belong to supergroup
Table retail_db.categories stats: [numFiles=1, numRows=0, totalSize=1029, rawDataSize=0]
OK
Time taken: 1.833 seconds
Note: /tmp/sqoop-cloudera/compile/a4de06e7be51786e8e8e3c227022cf2c/customers.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.

Logging initialized using configuration in jar:file:/usr/lib/hive/lib/hive-common-1.1.0-cdh5.12.0.jar!/hive-log4j.properties
OK
Time taken: 0.957 seconds
Loading data to table retail_db.customers
chgrp: changing ownership of 'hdfs://quickstart.cloudera:8020/user/hive/warehouse/retail_db.db/customers': User does not belong to supergroup
Table retail_db.customers stats: [numFiles=1, numRows=0, totalSize=953525, rawDataSize=0]
OK
Time taken: 2.057 seconds
Note: /tmp/sqoop-cloudera/compile/a4de06e7be51786e8e8e3c227022cf2c/departments.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.

Logging initialized using configuration in jar:file:/usr/lib/hive/lib/hive-common-1.1.0-cdh5.12.0.jar!/hive-log4j.properties
OK
Time taken: 0.382 seconds
Loading data to table retail_db.departments
chgrp: changing ownership of 'hdfs://quickstart.cloudera:8020/user/hive/warehouse/retail_db.db/departments': User does not belong to supergroup
Table retail_db.departments stats: [numFiles=1, numRows=0, totalSize=60, rawDataSize=0]
OK
Time taken: 1.39 seconds
Note: /tmp/sqoop-cloudera/compile/a4de06e7be51786e8e8e3c227022cf2c/order_items.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.

Logging initialized using configuration in jar:file:/usr/lib/hive/lib/hive-common-1.1.0-cdh5.12.0.jar!/hive-log4j.properties
OK
Time taken: 0.834 seconds
Loading data to table retail_db.order_items
chgrp: changing ownership of 'hdfs://quickstart.cloudera:8020/user/hive/warehouse/retail_db.db/order_items': User does not belong to supergroup
Table retail_db.order_items stats: [numFiles=1, numRows=0, totalSize=5408880, rawDataSize=0]
OK
Time taken: 1.149 seconds
Note: /tmp/sqoop-cloudera/compile/a4de06e7be51786e8e8e3c227022cf2c/orders.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.

Logging initialized using configuration in jar:file:/usr/lib/hive/lib/hive-common-1.1.0-cdh5.12.0.jar!/hive-log4j.properties
OK
Time taken: 0.51 seconds
Loading data to table retail_db.orders
chgrp: changing ownership of 'hdfs://quickstart.cloudera:8020/user/hive/warehouse/retail_db.db/orders': User does not belong to supergroup
Table retail_db.orders stats: [numFiles=1, numRows=0, totalSize=2999944, rawDataSize=0]
OK
Time taken: 1.554 seconds
Note: /tmp/sqoop-cloudera/compile/a4de06e7be51786e8e8e3c227022cf2c/products.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.

Logging initialized using configuration in jar:file:/usr/lib/hive/lib/hive-common-1.1.0-cdh5.12.0.jar!/hive-log4j.properties
OK
Time taken: 0.627 seconds
Loading data to table retail_db.products
chgrp: changing ownership of 'hdfs://quickstart.cloudera:8020/user/hive/warehouse/retail_db.db/products': User does not belong to supergroup
Table retail_db.products stats: [numFiles=1, numRows=0, totalSize=173993, rawDataSize=0]
OK
Time taken: 0.988 seconds
```

<br>

Logo nos conectamos ao **Beeline** e ao **Hive**, e através do **"USE LOCACAO;"** e **"SHOW TABLES;"** podemos ver que as tabelas ainda não estão presentes... mas é porque estamos no database errado!

```
[cloudera@quickstart ~]$ beeline
Beeline version 1.1.0-cdh5.12.0 by Apache Hive
beeline> !connect jdbc:hive2://
scan complete in 5ms
Connecting to jdbc:hive2://
Enter username for jdbc:hive2://: 
Enter password for jdbc:hive2://: 
Connected to: Apache Hive (version 1.1.0-cdh5.12.0)
Driver: Hive JDBC (version 1.1.0-cdh5.12.0)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://> USE LOCACAO;
OK
No rows affected (3.422 seconds)
0: jdbc:hive2://> SHOW TABLES;
OK
+---------------+--+
|   tab_name    |
+---------------+--+
| clientes      |
| despachantes  |
| locacao       |
| veiculos      |
+---------------+--+
4 rows selected (1.31 seconds)
```

<br>

Agora, com o comando **"INSERT OVERWRITE DIRECTORY '/user/cloudera/locacao2' SELECT * FROM LOCACAO;"** crio outro diretório com uma cópia de segurança dos arquivos contidos no diretorio locacao.
Em seguida, ainda no diretório locacao, com o comando **"SHOW DATABASES;"** conseguimos ver que o database retail_db está presente (= 

```
0: jdbc:hive2://> INSERT OVERWRITE DIRECTORY '/user/cloudera/locacao2' SELECT * FROM LOCACAO;
Query ID = cloudera_20221226051717_41453587-a0f0-4735-850e-cdeffe49972b
Total jobs = 3
Launching Job 1 out of 3
Number of reduce tasks is set to 0 since there's no reduce operator
22/12/26 05:17:09 [HiveServer2-Background-Pool: Thread-32]: WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
Starting Job = job_1671629034454_0020, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1671629034454_0020/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1671629034454_0020
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 0
22/12/26 05:17:39 [HiveServer2-Background-Pool: Thread-32]: WARN mapreduce.Counters: Group org.apache.hadoop.mapred.Task$Counter is deprecated. Use org.apache.hadoop.mapreduce.TaskCounter instead
2022-12-26 05:17:39,490 Stage-1 map = 0%,  reduce = 0%
2022-12-26 05:18:07,066 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 2.95 sec
MapReduce Total cumulative CPU time: 2 seconds 950 msec
Ended Job = job_1671629034454_0020
Stage-3 is selected by condition resolver.
Stage-2 is filtered out by condition resolver.
Stage-4 is filtered out by condition resolver.
Moving data to: hdfs://quickstart.cloudera:8020/user/cloudera/locacao2/.hive-staging_hive_2022-12-26_05-17-02_716_3230518618729584181-1/-ext-10000
Moving data to: /user/cloudera/locacao2
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1   Cumulative CPU: 2.95 sec   HDFS Read: 7306 HDFS Write: 3930 SUCCESS
Total MapReduce CPU Time Spent: 2 seconds 950 msec
OK
No rows affected (67.008 seconds)
0: jdbc:hive2://> SHOW DATABASES;
OK
+----------------+--+
| database_name  |
+----------------+--+
| default        |
| locacao        |
| retail_db      |
+----------------+--+
3 rows selected (0.54 seconds)
0: jdbc:hive2://> 
```



