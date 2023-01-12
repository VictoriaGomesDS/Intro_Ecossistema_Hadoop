# Explicando o desenvolvimento do Script "Hive_com_Spark" :pencil:

Esse foi o script onde mostro um pouco da comparação da diferença do tempo de execução de scripts identicos mas com a engine de execução Spark e Map Reduce, a default do Hive.

<br>

> Podemos identificar como os comandos que inseri aquelas instruções que estão a frente do indicador "0: jdbc:hive2://> ".
> <br>
> O proposito aqui é **mostrar a diferença no tempo de execução de dois scripts idênticos,** mas com a engine de execução Spark e Map Reduce.

<br>

Primeiramente, já logada no Hive e no database locacao, faço uma query conferindo que a engine de execução do Hive está como MR.
Então, seleciono diferente colunas das tabelas locacao_orc com o join da clientes_orc. 

<br>

Ao final podemos ver que o tempo de execução foi de **67.237 segundos** com a engine **Map Reduce**.

```
0: jdbc:hive2://> USE locacao;
OK
No rows affected (1.522 seconds)
0: jdbc:hive2://> set hive.execution.engine;
+---------------------------+--+
|            set            |
+---------------------------+--+
| hive.execution.engine=mr  |
+---------------------------+--+
1 row selected (0.179 seconds)
0: jdbc:hive2://> SELECT loc.datalocacao, loc.total, cli.nome FROM locacao_orc loc
. . . . . . . . > JOIN clientes_orc cli ON (loc.idcliente = cli.idcliente);
Query ID = cloudera_20230109152424_ee606683-cdaf-4117-a8cf-b8af2ca83d64
Total jobs = 1
Execution log at: /tmp/cloudera/cloudera_20230109152424_ee606683-cdaf-4117-a8cf-b8af2ca83d64.log
2023-01-09 03:24:33	Starting to launch local task to process map join;	maximum memory = 932184064
Execution log at: /tmp/cloudera/cloudera_20230109152424_ee606683-cdaf-4117-a8cf-b8af2ca83d64.log
2023-01-09 03:24:33	Starting to launch local task to process map join;	maximum memory = 932184064
2023-01-09 03:24:39	Dump the side-table for tag: 0 with group count: 25 into file: file:/tmp/cloudera/4497e653-dacd-4053-9373-6687d142c498/hive_2023-01-09_15-24-15_054_2959333596181646190-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile00--.hashtable
2023-01-09 03:24:39	Uploaded 1 File to: file:/tmp/cloudera/4497e653-dacd-4053-9373-6687d142c498/hive_2023-01-09_15-24-15_054_2959333596181646190-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile00--.hashtable (2218 bytes)
2023-01-09 03:24:39	End of local task; Time Taken: 6.493 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks is set to 0 since there's no reduce operator
23/01/09 15:24:42 [HiveServer2-Background-Pool: Thread-29]: WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
2023-01-09 03:24:39	Dump the side-table for tag: 0 with group count: 25 into file: file:/tmp/cloudera/4497e653-dacd-4053-9373-6687d142c498/hive_2023-01-09_15-24-15_054_2959333596181646190-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile00--.hashtable
2023-01-09 03:24:39	Uploaded 1 File to: file:/tmp/cloudera/4497e653-dacd-4053-9373-6687d142c498/hive_2023-01-09_15-24-15_054_2959333596181646190-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile00--.hashtable (2218 bytes)
2023-01-09 03:24:39	End of local task; Time Taken: 6.493 sec.
Starting Job = job_1673303255649_0004, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1673303255649_0004/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1673303255649_0004
Hadoop job information for Stage-3: number of mappers: 1; number of reducers: 0
23/01/09 15:24:57 [HiveServer2-Background-Pool: Thread-29]: WARN mapreduce.Counters: Group org.apache.hadoop.mapred.Task$Counter is deprecated. Use org.apache.hadoop.mapreduce.TaskCounter instead
2023-01-09 15:24:57,884 Stage-3 map = 0%,  reduce = 0%
2023-01-09 15:25:19,736 Stage-3 map = 100%,  reduce = 0%, Cumulative CPU 1.93 sec
MapReduce Total cumulative CPU time: 1 seconds 930 msec
Ended Job = job_1673303255649_0004
MapReduce Jobs Launched: 
Stage-Stage-3: Map: 1   Cumulative CPU: 1.93 sec   HDFS Read: 9233 HDFS Write: 3473 SUCCESS
Total MapReduce CPU Time Spent: 1 seconds 930 msec
OK
+------------------+------------+---------------------+--+
| loc.datalocacao  | loc.total  |      cli.nome       |
+------------------+------------+---------------------+--+
| 2019-03-17       | 2010.0     | Alvito Espargosa    |
| 2019-06-17       | 2020.0     | Alvito Espargosa    |
| 2019-03-08       | 1975.0     | Alvito Espargosa    |
| 2019-03-30       | 1886.0     | Alvito Espargosa    |
| 2019-06-22       | 2070.0     | Blasco Canto        |
| 2019-02-20       | 1996.0     | Carminda Veríssimo  |
| 2019-05-11       | 2095.0     | Carminda Veríssimo  |
| 2019-03-18       | 1914.0     | Carminda Veríssimo  |
| 2019-04-09       | 1810.0     | Cleusa Lamego       |
| 2019-05-21       | 2009.0     | Cleusa Lamego       |
| 2019-04-11       | 2001.0     | Cleusa Lamego       |
| 2019-06-30       | 1843.0     | Clóvis Carrasco     |
| 2019-06-01       | 1980.0     | Clóvis Carrasco     |
| 2019-05-21       | 1974.0     | Clóvis Carrasco     |
| 2019-06-22       | 1860.0     | Clóvis Carrasco     |
| 2019-04-15       | 1845.0     | Cátia Fróis         |
| 2019-05-28       | 1818.0     | Cátia Fróis         |
| 2019-06-08       | 1853.0     | Cátia Fróis         |
| 2019-06-23       | 1831.0     | Cátia Fróis         |
| 2019-04-16       | 1956.0     | Cátia Fróis         |
| 2019-05-02       | 1900.0     | Emílio Faro         |
| 2019-04-21       | 1890.0     | Emílio Faro         |
| 2019-06-14       | 1813.0     | Emílio Faro         |
| 2019-05-13       | 1843.0     | Emílio Faro         |
| 2019-04-28       | 1899.0     | Emílio Faro         |
| 2019-03-27       | 1967.0     | Emílio Faro         |
| 2019-02-24       | 2049.0     | Hipólito Granja     |
| 2019-05-06       | 1865.0     | Hipólito Granja     |
| 2019-05-29       | 2054.0     | Hipólito Granja     |
| 2019-04-21       | 2017.0     | Iara Cardoso        |
| 2019-03-04       | 1821.0     | Iara Cardoso        |
| 2019-06-04       | 1938.0     | Iara Cardoso        |
| 2019-03-08       | 2075.0     | Iara Cardoso        |
| 2019-04-03       | 1945.0     | Iara Cardoso        |
| 2019-02-27       | 1851.0     | Iara Cardoso        |
| 2019-02-22       | 2071.0     | Isaura Farias       |
| 2019-05-12       | 2006.0     | Iuri Alancastre     |
| 2019-03-21       | 2037.0     | Iuri Alancastre     |
| 2019-04-20       | 1800.0     | Iuri Alancastre     |
| 2019-02-20       | 1858.0     | Iuri Alancastre     |
| 2019-03-27       | 1950.0     | Iuri Alancastre     |
| 2019-04-01       | 1990.0     | Iuri Alancastre     |
| 2019-03-22       | 1802.0     | Iuri Alancastre     |
| 2019-04-20       | 1946.0     | Laura Marcondes     |
| 2019-05-01       | 1989.0     | Micael Mangueira    |
| 2019-04-12       | 2077.0     | Micael Mangueira    |
| 2019-04-06       | 1855.0     | Micael Mangueira    |
| 2019-05-06       | 2011.0     | Micael Mangueira    |
| 2019-06-28       | 2020.0     | Micael Mangueira    |
| 2019-06-08       | 1971.0     | Micael Mangueira    |
| 2019-03-05       | 2039.0     | Micael Mangueira    |
| 2019-03-10       | 2056.0     | Micael Mangueira    |
| 2019-05-15       | 1986.0     | Márcio Taveira      |
| 2019-03-26       | 1867.0     | Márcio Taveira      |
| 2019-06-05       | 1997.0     | Noêmia   Tupinambá  |
| 2019-06-22       | 1938.0     | Noêmia   Tupinambá  |
| 2019-02-22       | 2031.0     | Noêmia   Tupinambá  |
| 2019-03-27       | 2004.0     | Noêmia   Tupinambá  |
| 2019-06-12       | 1939.0     | Noêmia   Tupinambá  |
| 2019-06-02       | 1867.0     | Paula Padilha       |
| 2019-03-18       | 1991.0     | Paula Padilha       |
| 2019-05-26       | 1983.0     | Paula Padilha       |
| 2019-06-15       | 1812.0     | Paula Padilha       |
| 2019-06-22       | 2089.0     | Paula Padilha       |
| 2019-02-27       | 2100.0     | Paula Padilha       |
| 2019-03-09       | 2016.0     | Rebeca Torcuato     |
| 2019-05-10       | 2027.0     | Rebeca Torcuato     |
| 2019-05-29       | 2035.0     | Rebeca Torcuato     |
| 2019-02-20       | 2042.0     | Rebeca Torcuato     |
| 2019-03-15       | 1958.0     | Rebeca Torcuato     |
| 2019-05-19       | 1810.0     | Rosana Betancour    |
| 2019-05-22       | 1986.0     | Rosana Betancour    |
| 2019-02-26       | 1915.0     | Sabino Abrantes     |
| 2019-05-12       | 1862.0     | Sabino Abrantes     |
| 2019-03-14       | 1963.0     | Sabino Abrantes     |
| 2019-06-12       | 2089.0     | Severino Leiria     |
| 2019-04-02       | 1837.0     | Severino Leiria     |
| 2019-06-03       | 2049.0     | Severino Leiria     |
| 2019-04-26       | 2033.0     | Tobias Garcés       |
| 2019-06-15       | 1888.0     | Tobias Garcés       |
| 2019-05-11       | 2024.0     | Tobias Garcés       |
| 2019-05-07       | 1826.0     | Tobias Garcés       |
| 2019-03-29       | 1874.0     | Vanderlei Açores    |
| 2019-05-07       | 2032.0     | Vanderlei Açores    |
| 2019-05-22       | 1836.0     | Vanderlei Açores    |
| 2019-04-08       | 1921.0     | Vanderlei Açores    |
| 2019-02-21       | 1861.0     | Vanderlei Açores    |
| 2019-02-27       | 2014.0     | Vicente Rosa        |
| 2019-05-07       | 1855.0     | Vicente Rosa        |
| 2019-06-21       | 1840.0     | Vicente Rosa        |
| 2019-05-10       | 1838.0     | Vicente Rosa        |
| 2019-02-26       | 2064.0     | Vicente Rosa        |
| 2019-04-05       | 1927.0     | Vicente Rosa        |
| 2019-04-23       | 1801.0     | Vicente Rosa        |
| 2019-04-19       | 1857.0     | Xisto Mendoça       |
| 2019-06-04       | 1942.0     | Xisto Mendoça       |
| 2019-05-31       | 1970.0     | Xisto Mendoça       |
| 2019-06-26       | 1881.0     | Xisto Mendoça       |
| 2019-05-13       | 1864.0     | Xisto Mendoça       |
| 2019-03-16       | 2034.0     | Zeferino Matoso     |
+------------------+------------+---------------------+--+
100 rows selected (67.237 seconds)
```

<br>
<br>

Agora faço uma query fazendo um setda engine de execução do Hive do MR para Spark.
Então, seleciono diferente colunas das tabelas locacao_orc com o join da clientes_orc. 

<br>

Ao final podemos ver que o tempo de execução foi de **171.208 segundos** com a engine **Spark**. ***Como assim se dizem que o spark (geralmente) é 3X "mais rápido"?*** :frowning:

<br>

```
0: jdbc:hive2://> set hive.execution.engine=spark;
No rows affected (0.008 seconds)
0: jdbc:hive2://> set hive.execution.engine;
+------------------------------+--+
|             set              |
+------------------------------+--+
| hive.execution.engine=spark  |
+------------------------------+--+
1 row selected (0.003 seconds)
0: jdbc:hive2://> SELECT loc.datalocacao, loc.total, cli.nome FROM locacao_orc loc
. . . . . . . . > JOIN clientes_orc cli ON (loc.idcliente = cli.idcliente);
Query ID = cloudera_20230109152626_c04d9d34-78ce-4c05-9eba-6397e6dd071a
Total jobs = 2
Launching Job 1 out of 2
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Spark Job = b4e844b9-fc8d-4e3c-a047-c2cb334339a0
Running with YARN Application = application_1673303255649_0005
Kill Command = /usr/lib/hadoop/bin/yarn application -kill application_1673303255649_0005

Query Hive on Spark job[0] stages:
0

Status: Running (Hive on Spark job[0])
Job Progress Format
CurrentTime StageId_StageAttemptId: SucceededTasksCount(+RunningTasksCount-FailedTasksCount)/TotalTasksCount [StageCost]
2023-01-09 15:27:38,930	Stage-0_0: 0/1	
2023-01-09 15:27:41,291	Stage-0_0: 0(+1)/1	
2023-01-09 15:27:44,685	Stage-0_0: 0(+1)/1	
2023-01-09 15:27:47,929	Stage-0_0: 0(+1)/1	
2023-01-09 15:27:51,032	Stage-0_0: 0(+1)/1	
2023-01-09 15:27:54,371	Stage-0_0: 0(+1)/1	
2023-01-09 15:27:57,390	Stage-0_0: 0(+1)/1	
2023-01-09 15:28:00,417	Stage-0_0: 0(+1)/1	
2023-01-09 15:28:03,452	Stage-0_0: 0(+1)/1	
2023-01-09 15:28:06,534	Stage-0_0: 0(+1)/1	
2023-01-09 15:28:09,562	Stage-0_0: 0(+1)/1	
2023-01-09 15:28:12,662	Stage-0_0: 0(+1)/1	
2023-01-09 15:28:15,875	Stage-0_0: 0(+1)/1	
2023-01-09 15:28:17,179	Stage-0_0: 1/1 Finished	
2023-01-09 15:28:21,797	Stage-0_0: 1/1 Finished	
Status: Finished successfully in 70.02 seconds
Launching Job 2 out of 2
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Spark Job = a4e79da5-2c00-4602-8692-e5804c27ba74
Running with YARN Application = application_1673303255649_0005
Kill Command = /usr/lib/hadoop/bin/yarn application -kill application_1673303255649_0005

Query Hive on Spark job[1] stages:
1

Status: Running (Hive on Spark job[1])
Job Progress Format
CurrentTime StageId_StageAttemptId: SucceededTasksCount(+RunningTasksCount-FailedTasksCount)/TotalTasksCount [StageCost]
2023-01-09 15:28:28,416	Stage-1_0: 0(+1)/1	
2023-01-09 15:28:31,514	Stage-1_0: 0(+1)/1	
2023-01-09 15:28:34,653	Stage-1_0: 0(+1)/1	
2023-01-09 15:28:37,985	Stage-1_0: 0(+1)/1	
2023-01-09 15:28:41,208	Stage-1_0: 0(+1)/1	
2023-01-09 15:28:44,470	Stage-1_0: 0(+1)/1	
2023-01-09 15:28:47,516	Stage-1_0: 0(+1)/1	
2023-01-09 15:28:51,342	Stage-1_0: 0(+1)/1	
2023-01-09 15:28:53,395	Stage-1_0: 1/1 Finished	
Status: Finished successfully in 28.03 seconds
OK
+------------------+------------+---------------------+--+
| loc.datalocacao  | loc.total  |      cli.nome       |
+------------------+------------+---------------------+--+
| 2019-03-17       | 2010.0     | Alvito Espargosa    |
| 2019-06-17       | 2020.0     | Alvito Espargosa    |
| 2019-03-08       | 1975.0     | Alvito Espargosa    |
| 2019-03-30       | 1886.0     | Alvito Espargosa    |
| 2019-06-22       | 2070.0     | Blasco Canto        |
| 2019-02-20       | 1996.0     | Carminda Veríssimo  |
| 2019-05-11       | 2095.0     | Carminda Veríssimo  |
| 2019-03-18       | 1914.0     | Carminda Veríssimo  |
| 2019-04-09       | 1810.0     | Cleusa Lamego       |
| 2019-05-21       | 2009.0     | Cleusa Lamego       |
| 2019-04-11       | 2001.0     | Cleusa Lamego       |
| 2019-06-30       | 1843.0     | Clóvis Carrasco     |
| 2019-06-01       | 1980.0     | Clóvis Carrasco     |
| 2019-05-21       | 1974.0     | Clóvis Carrasco     |
| 2019-06-22       | 1860.0     | Clóvis Carrasco     |
| 2019-04-15       | 1845.0     | Cátia Fróis         |
| 2019-05-28       | 1818.0     | Cátia Fróis         |
| 2019-06-08       | 1853.0     | Cátia Fróis         |
| 2019-06-23       | 1831.0     | Cátia Fróis         |
| 2019-04-16       | 1956.0     | Cátia Fróis         |
| 2019-05-02       | 1900.0     | Emílio Faro         |
| 2019-04-21       | 1890.0     | Emílio Faro         |
| 2019-06-14       | 1813.0     | Emílio Faro         |
| 2019-05-13       | 1843.0     | Emílio Faro         |
| 2019-04-28       | 1899.0     | Emílio Faro         |
| 2019-03-27       | 1967.0     | Emílio Faro         |
| 2019-02-24       | 2049.0     | Hipólito Granja     |
| 2019-05-06       | 1865.0     | Hipólito Granja     |
| 2019-05-29       | 2054.0     | Hipólito Granja     |
| 2019-04-21       | 2017.0     | Iara Cardoso        |
| 2019-03-04       | 1821.0     | Iara Cardoso        |
| 2019-06-04       | 1938.0     | Iara Cardoso        |
| 2019-03-08       | 2075.0     | Iara Cardoso        |
| 2019-04-03       | 1945.0     | Iara Cardoso        |
| 2019-02-27       | 1851.0     | Iara Cardoso        |
| 2019-02-22       | 2071.0     | Isaura Farias       |
| 2019-05-12       | 2006.0     | Iuri Alancastre     |
| 2019-03-21       | 2037.0     | Iuri Alancastre     |
| 2019-04-20       | 1800.0     | Iuri Alancastre     |
| 2019-02-20       | 1858.0     | Iuri Alancastre     |
| 2019-03-27       | 1950.0     | Iuri Alancastre     |
| 2019-04-01       | 1990.0     | Iuri Alancastre     |
| 2019-03-22       | 1802.0     | Iuri Alancastre     |
| 2019-04-20       | 1946.0     | Laura Marcondes     |
| 2019-05-01       | 1989.0     | Micael Mangueira    |
| 2019-04-12       | 2077.0     | Micael Mangueira    |
| 2019-04-06       | 1855.0     | Micael Mangueira    |
| 2019-05-06       | 2011.0     | Micael Mangueira    |
| 2019-06-28       | 2020.0     | Micael Mangueira    |
| 2019-06-08       | 1971.0     | Micael Mangueira    |
| 2019-03-05       | 2039.0     | Micael Mangueira    |
| 2019-03-10       | 2056.0     | Micael Mangueira    |
| 2019-05-15       | 1986.0     | Márcio Taveira      |
| 2019-03-26       | 1867.0     | Márcio Taveira      |
| 2019-06-05       | 1997.0     | Noêmia   Tupinambá  |
| 2019-06-22       | 1938.0     | Noêmia   Tupinambá  |
| 2019-02-22       | 2031.0     | Noêmia   Tupinambá  |
| 2019-03-27       | 2004.0     | Noêmia   Tupinambá  |
| 2019-06-12       | 1939.0     | Noêmia   Tupinambá  |
| 2019-06-02       | 1867.0     | Paula Padilha       |
| 2019-03-18       | 1991.0     | Paula Padilha       |
| 2019-05-26       | 1983.0     | Paula Padilha       |
| 2019-06-15       | 1812.0     | Paula Padilha       |
| 2019-06-22       | 2089.0     | Paula Padilha       |
| 2019-02-27       | 2100.0     | Paula Padilha       |
| 2019-03-09       | 2016.0     | Rebeca Torcuato     |
| 2019-05-10       | 2027.0     | Rebeca Torcuato     |
| 2019-05-29       | 2035.0     | Rebeca Torcuato     |
| 2019-02-20       | 2042.0     | Rebeca Torcuato     |
| 2019-03-15       | 1958.0     | Rebeca Torcuato     |
| 2019-05-19       | 1810.0     | Rosana Betancour    |
| 2019-05-22       | 1986.0     | Rosana Betancour    |
| 2019-02-26       | 1915.0     | Sabino Abrantes     |
| 2019-05-12       | 1862.0     | Sabino Abrantes     |
| 2019-03-14       | 1963.0     | Sabino Abrantes     |
| 2019-06-12       | 2089.0     | Severino Leiria     |
| 2019-04-02       | 1837.0     | Severino Leiria     |
| 2019-06-03       | 2049.0     | Severino Leiria     |
| 2019-04-26       | 2033.0     | Tobias Garcés       |
| 2019-06-15       | 1888.0     | Tobias Garcés       |
| 2019-05-11       | 2024.0     | Tobias Garcés       |
| 2019-05-07       | 1826.0     | Tobias Garcés       |
| 2019-03-29       | 1874.0     | Vanderlei Açores    |
| 2019-05-07       | 2032.0     | Vanderlei Açores    |
| 2019-05-22       | 1836.0     | Vanderlei Açores    |
| 2019-04-08       | 1921.0     | Vanderlei Açores    |
| 2019-02-21       | 1861.0     | Vanderlei Açores    |
| 2019-02-27       | 2014.0     | Vicente Rosa        |
| 2019-05-07       | 1855.0     | Vicente Rosa        |
| 2019-06-21       | 1840.0     | Vicente Rosa        |
| 2019-05-10       | 1838.0     | Vicente Rosa        |
| 2019-02-26       | 2064.0     | Vicente Rosa        |
| 2019-04-05       | 1927.0     | Vicente Rosa        |
| 2019-04-23       | 1801.0     | Vicente Rosa        |
| 2019-04-19       | 1857.0     | Xisto Mendoça       |
| 2019-06-04       | 1942.0     | Xisto Mendoça       |
| 2019-05-31       | 1970.0     | Xisto Mendoça       |
| 2019-06-26       | 1881.0     | Xisto Mendoça       |
| 2019-05-13       | 1864.0     | Xisto Mendoça       |
| 2019-03-16       | 2034.0     | Zeferino Matoso     |
+------------------+------------+---------------------+--+
100 rows selected (171.208 seconds)
```

<br>
<br>

> Essa foi uma query de "ativação do spark como engine", por isso houve essa "demora". 
> Que tal fazermos a mesma query, mas com o spark já inicializado para vermos a diferença?

<br>
<br>

Agora com o spark já aconchegado e ligado no role podemos executar a mesma query novamente hahaha.
Então, seleciono diferente colunas das tabelas locacao_orc com o join da clientes_orc. 
<br>
Ao final podemos ver que o tempo de execução foi de **18.439 segundos** com a engine **Spark**. Cerca de 3X mais rápido que 67.237 segundos.

```
0: jdbc:hive2://> SELECT loc.datalocacao, loc.total, cli.nome FROM locacao_orc loc
. . . . . . . . > JOIN clientes_orc cli ON (loc.idcliente = cli.idcliente);
Query ID = cloudera_20230109152929_9ade2522-a7a7-41a8-985a-7b8e100a2322
Total jobs = 2
Launching Job 1 out of 2
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Spark Job = 9ac001d3-f06f-4529-acb8-5b50f533278f
Running with YARN Application = application_1673303255649_0005
Kill Command = /usr/lib/hadoop/bin/yarn application -kill application_1673303255649_0005

Query Hive on Spark job[2] stages:
2

Status: Running (Hive on Spark job[2])
Job Progress Format
CurrentTime StageId_StageAttemptId: SucceededTasksCount(+RunningTasksCount-FailedTasksCount)/TotalTasksCount [StageCost]
2023-01-09 15:29:40,908	Stage-2_0: 0(+1)/1	
2023-01-09 15:29:41,916	Stage-2_0: 1/1 Finished	
Status: Finished successfully in 3.06 seconds
Launching Job 2 out of 2
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Spark Job = eb83c615-14dd-4ec9-a13a-cc7c0d33e6a3
Running with YARN Application = application_1673303255649_0005
Kill Command = /usr/lib/hadoop/bin/yarn application -kill application_1673303255649_0005

Query Hive on Spark job[3] stages:
3

Status: Running (Hive on Spark job[3])
Job Progress Format
CurrentTime StageId_StageAttemptId: SucceededTasksCount(+RunningTasksCount-FailedTasksCount)/TotalTasksCount [StageCost]
2023-01-09 15:29:43,065	Stage-3_0: 0(+1)/1	
2023-01-09 15:29:44,068	Stage-3_0: 1/1 Finished	
Status: Finished successfully in 2.01 seconds
OK
+------------------+------------+---------------------+--+
| loc.datalocacao  | loc.total  |      cli.nome       |
+------------------+------------+---------------------+--+
| 2019-03-17       | 2010.0     | Alvito Espargosa    |
| 2019-06-17       | 2020.0     | Alvito Espargosa    |
| 2019-03-08       | 1975.0     | Alvito Espargosa    |
| 2019-03-30       | 1886.0     | Alvito Espargosa    |
| 2019-06-22       | 2070.0     | Blasco Canto        |
| 2019-02-20       | 1996.0     | Carminda Veríssimo  |
| 2019-05-11       | 2095.0     | Carminda Veríssimo  |
| 2019-03-18       | 1914.0     | Carminda Veríssimo  |
| 2019-04-09       | 1810.0     | Cleusa Lamego       |
| 2019-05-21       | 2009.0     | Cleusa Lamego       |
| 2019-04-11       | 2001.0     | Cleusa Lamego       |
| 2019-06-30       | 1843.0     | Clóvis Carrasco     |
| 2019-06-01       | 1980.0     | Clóvis Carrasco     |
| 2019-05-21       | 1974.0     | Clóvis Carrasco     |
| 2019-06-22       | 1860.0     | Clóvis Carrasco     |
| 2019-04-15       | 1845.0     | Cátia Fróis         |
| 2019-05-28       | 1818.0     | Cátia Fróis         |
| 2019-06-08       | 1853.0     | Cátia Fróis         |
| 2019-06-23       | 1831.0     | Cátia Fróis         |
| 2019-04-16       | 1956.0     | Cátia Fróis         |
| 2019-05-02       | 1900.0     | Emílio Faro         |
| 2019-04-21       | 1890.0     | Emílio Faro         |
| 2019-06-14       | 1813.0     | Emílio Faro         |
| 2019-05-13       | 1843.0     | Emílio Faro         |
| 2019-04-28       | 1899.0     | Emílio Faro         |
| 2019-03-27       | 1967.0     | Emílio Faro         |
| 2019-02-24       | 2049.0     | Hipólito Granja     |
| 2019-05-06       | 1865.0     | Hipólito Granja     |
| 2019-05-29       | 2054.0     | Hipólito Granja     |
| 2019-04-21       | 2017.0     | Iara Cardoso        |
| 2019-03-04       | 1821.0     | Iara Cardoso        |
| 2019-06-04       | 1938.0     | Iara Cardoso        |
| 2019-03-08       | 2075.0     | Iara Cardoso        |
| 2019-04-03       | 1945.0     | Iara Cardoso        |
| 2019-02-27       | 1851.0     | Iara Cardoso        |
| 2019-02-22       | 2071.0     | Isaura Farias       |
| 2019-05-12       | 2006.0     | Iuri Alancastre     |
| 2019-03-21       | 2037.0     | Iuri Alancastre     |
| 2019-04-20       | 1800.0     | Iuri Alancastre     |
| 2019-02-20       | 1858.0     | Iuri Alancastre     |
| 2019-03-27       | 1950.0     | Iuri Alancastre     |
| 2019-04-01       | 1990.0     | Iuri Alancastre     |
| 2019-03-22       | 1802.0     | Iuri Alancastre     |
| 2019-04-20       | 1946.0     | Laura Marcondes     |
| 2019-05-01       | 1989.0     | Micael Mangueira    |
| 2019-04-12       | 2077.0     | Micael Mangueira    |
| 2019-04-06       | 1855.0     | Micael Mangueira    |
| 2019-05-06       | 2011.0     | Micael Mangueira    |
| 2019-06-28       | 2020.0     | Micael Mangueira    |
| 2019-06-08       | 1971.0     | Micael Mangueira    |
| 2019-03-05       | 2039.0     | Micael Mangueira    |
| 2019-03-10       | 2056.0     | Micael Mangueira    |
| 2019-05-15       | 1986.0     | Márcio Taveira      |
| 2019-03-26       | 1867.0     | Márcio Taveira      |
| 2019-06-05       | 1997.0     | Noêmia   Tupinambá  |
| 2019-06-22       | 1938.0     | Noêmia   Tupinambá  |
| 2019-02-22       | 2031.0     | Noêmia   Tupinambá  |
| 2019-03-27       | 2004.0     | Noêmia   Tupinambá  |
| 2019-06-12       | 1939.0     | Noêmia   Tupinambá  |
| 2019-06-02       | 1867.0     | Paula Padilha       |
| 2019-03-18       | 1991.0     | Paula Padilha       |
| 2019-05-26       | 1983.0     | Paula Padilha       |
| 2019-06-15       | 1812.0     | Paula Padilha       |
| 2019-06-22       | 2089.0     | Paula Padilha       |
| 2019-02-27       | 2100.0     | Paula Padilha       |
| 2019-03-09       | 2016.0     | Rebeca Torcuato     |
| 2019-05-10       | 2027.0     | Rebeca Torcuato     |
| 2019-05-29       | 2035.0     | Rebeca Torcuato     |
| 2019-02-20       | 2042.0     | Rebeca Torcuato     |
| 2019-03-15       | 1958.0     | Rebeca Torcuato     |
| 2019-05-19       | 1810.0     | Rosana Betancour    |
| 2019-05-22       | 1986.0     | Rosana Betancour    |
| 2019-02-26       | 1915.0     | Sabino Abrantes     |
| 2019-05-12       | 1862.0     | Sabino Abrantes     |
| 2019-03-14       | 1963.0     | Sabino Abrantes     |
| 2019-06-12       | 2089.0     | Severino Leiria     |
| 2019-04-02       | 1837.0     | Severino Leiria     |
| 2019-06-03       | 2049.0     | Severino Leiria     |
| 2019-04-26       | 2033.0     | Tobias Garcés       |
| 2019-06-15       | 1888.0     | Tobias Garcés       |
| 2019-05-11       | 2024.0     | Tobias Garcés       |
| 2019-05-07       | 1826.0     | Tobias Garcés       |
| 2019-03-29       | 1874.0     | Vanderlei Açores    |
| 2019-05-07       | 2032.0     | Vanderlei Açores    |
| 2019-05-22       | 1836.0     | Vanderlei Açores    |
| 2019-04-08       | 1921.0     | Vanderlei Açores    |
| 2019-02-21       | 1861.0     | Vanderlei Açores    |
| 2019-02-27       | 2014.0     | Vicente Rosa        |
| 2019-05-07       | 1855.0     | Vicente Rosa        |
| 2019-06-21       | 1840.0     | Vicente Rosa        |
| 2019-05-10       | 1838.0     | Vicente Rosa        |
| 2019-02-26       | 2064.0     | Vicente Rosa        |
| 2019-04-05       | 1927.0     | Vicente Rosa        |
| 2019-04-23       | 1801.0     | Vicente Rosa        |
| 2019-04-19       | 1857.0     | Xisto Mendoça       |
| 2019-06-04       | 1942.0     | Xisto Mendoça       |
| 2019-05-31       | 1970.0     | Xisto Mendoça       |
| 2019-06-26       | 1881.0     | Xisto Mendoça       |
| 2019-05-13       | 1864.0     | Xisto Mendoça       |
| 2019-03-16       | 2034.0     | Zeferino Matoso     |
+------------------+------------+---------------------+--+
100 rows selected (18.439 seconds)
```


<br>
<br>

Finalizo a descrição desse script aqui. Acrescento que são minhas observações durante meu aprendizado e que ainda estou no início, então peço paciência sobre qualquer erro (= Obrigada pela compreensão :smile:

Obrigada por ler até o final! :wink:




