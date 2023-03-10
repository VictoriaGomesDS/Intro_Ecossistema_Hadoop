# Explicando o desenvolvimento do Script "Vetorizacao" :pencil:

Esse foi o script onde mostro um pouco da diferença do tempo de execução de scripts identicos mas com a vetorização ativada e desativada.

<br>

![image](https://user-images.githubusercontent.com/102270053/213742303-5cfeda4d-7722-435c-a7bb-e6d8d3118867.png)

<br>
<br>

## Overview Vetorização

A **vetorização** é, cruamente falando, uma "técnica" de otimização de processamento, onde ela busca reduzir o uso de CPU em consultas através da execução do processamento de blocos de 1.024 linhas por vez em vez de 1. Permite o processamento em batch.

É utilizada em querys em arquivos ORC.

<br>
<br>

## Analise do código

> Podemos identificar como os comandos que inseri aquelas instruções que estão a frente do indicador "0: jdbc:hive2://> ".
> O proposito aqui é mostrar a diferença no tempo de execução de dois scripts idênticos, mas com a vetorização ativada e desativada.

<br>

Abaixo, no shell do Hive, nós nos conectamos ao database "locacao", criamos uma tabela chamada locacao_orc, no modelo orc, e a populamos com os dados da tabela locacao.

```
0: jdbc:hive2://> USE locacao;
OK
No rows affected (2.349 seconds)
0: jdbc:hive2://> CREATE EXTERNAL TABLE locacao_orc (idlocacao int, idcliente int, iddespachante int, idveiculo int, datalocacao date, dataentrega date, total double)
. . . . . . . . > STORED AS orc;
OK
No rows affected (0.886 seconds)
0: jdbc:hive2://> INSERT OVERWRITE TABLE locacao_orc SELECT * FROM locacao;
Query ID = cloudera_20230109111616_18008d60-23ae-4638-9c12-e9600f4fb2e9
Total jobs = 1
Launching Job 1 out of 1
Number of reduce tasks is set to 0 since there's no reduce operator
23/01/09 11:16:09 [HiveServer2-Background-Pool: Thread-35]: WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
Starting Job = job_1671629034454_0024, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1671629034454_0024/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1671629034454_0024
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 0
23/01/09 11:16:25 [HiveServer2-Background-Pool: Thread-35]: WARN mapreduce.Counters: Group org.apache.hadoop.mapred.Task$Counter is deprecated. Use org.apache.hadoop.mapreduce.TaskCounter instead
2023-01-09 11:16:25,432 Stage-1 map = 0%,  reduce = 0%
2023-01-09 11:16:45,786 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 4.73 sec
MapReduce Total cumulative CPU time: 4 seconds 730 msec
Ended Job = job_1671629034454_0024
Stage-4 is selected by condition resolver.
Stage-3 is filtered out by condition resolver.
Stage-5 is filtered out by condition resolver.
Moving data to: hdfs://quickstart.cloudera:8020/user/hive/warehouse/locacao.db/locacao_orc/.hive-staging_hive_2023-01-09_11-16-05_791_5659779996369571459-1/-ext-10000
Loading data to table locacao.locacao_orc
Table locacao.locacao_orc stats: [numFiles=1, numRows=100, totalSize=1490, rawDataSize=13600]
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1   Cumulative CPU: 4.73 sec   HDFS Read: 8171 HDFS Write: 1569 SUCCESS
Total MapReduce CPU Time Spent: 4 seconds 730 msec
OK
No rows affected (45.53 seconds)
```

<br>
<br>

Em seguida faço uma query selecionando algumas colunas da tabela locacao_orc e clientes, então faço join com a tabela clientes.
Ao final da execução podemos ver que ela levou cerca de **76.66 segundos**. Aqui, a vetorização ainda está **desativada**.

```
0: jdbc:hive2://> SELECT loc.datalocacao, loc.total, cli.nome FROM locacao_orc loc
. . . . . . . . > JOIN clientes_orc cli ON (loc.idcliente = cli.idcliente);
Query ID = cloudera_20230109112020_84304089-d578-4ca3-ba35-260e2f9b6319
Total jobs = 1
Execution log at: /tmp/cloudera/cloudera_20230109112020_84304089-d578-4ca3-ba35-260e2f9b6319.log
Execution log at: /tmp/cloudera/cloudera_20230109112020_84304089-d578-4ca3-ba35-260e2f9b6319.log
2023-01-09 11:20:26	Starting to launch local task to process map join;	maximum memory = 932184064
2023-01-09 11:20:31	Dump the side-table for tag: 0 with group count: 25 into file: file:/tmp/cloudera/e20f8781-074c-4a57-81e0-c9a6c48e9dbc/hive_2023-01-09_11-20-14_852_8276511922395874874-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile00--.hashtable
2023-01-09 11:20:31	Uploaded 1 File to: file:/tmp/cloudera/e20f8781-074c-4a57-81e0-c9a6c48e9dbc/hive_2023-01-09_11-20-14_852_8276511922395874874-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile00--.hashtable (2218 bytes)
2023-01-09 11:20:31	End of local task; Time Taken: 5.479 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks is set to 0 since there's no reduce operator
23/01/09 11:20:34 [HiveServer2-Background-Pool: Thread-65]: WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
Starting Job = job_1671629034454_0025, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1671629034454_0025/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1671629034454_0025
2023-01-09 11:20:26	Starting to launch local task to process map join;	maximum memory = 932184064
2023-01-09 11:20:31	Dump the side-table for tag: 0 with group count: 25 into file: file:/tmp/cloudera/e20f8781-074c-4a57-81e0-c9a6c48e9dbc/hive_2023-01-09_11-20-14_852_8276511922395874874-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile00--.hashtable
2023-01-09 11:20:31	Uploaded 1 File to: file:/tmp/cloudera/e20f8781-074c-4a57-81e0-c9a6c48e9dbc/hive_2023-01-09_11-20-14_852_8276511922395874874-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile00--.hashtable (2218 bytes)
2023-01-09 11:20:31	End of local task; Time Taken: 5.479 sec.
Hadoop job information for Stage-3: number of mappers: 1; number of reducers: 0
23/01/09 11:20:56 [HiveServer2-Background-Pool: Thread-65]: WARN mapreduce.Counters: Group org.apache.hadoop.mapred.Task$Counter is deprecated. Use org.apache.hadoop.mapreduce.TaskCounter instead
2023-01-09 11:20:56,133 Stage-3 map = 0%,  reduce = 0%
2023-01-09 11:21:27,647 Stage-3 map = 100%,  reduce = 0%, Cumulative CPU 9.09 sec
MapReduce Total cumulative CPU time: 9 seconds 90 msec
Ended Job = job_1671629034454_0025
MapReduce Jobs Launched: 
Stage-Stage-3: Map: 1   Cumulative CPU: 9.09 sec   HDFS Read: 9260 HDFS Write: 3473 SUCCESS
Total MapReduce CPU Time Spent: 9 seconds 90 msec
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
100 rows selected (76.66 seconds)
```

<br>
<br>

Como indicado anteriormente, agora podemos ver que a vetorização está desativada **"hive.vectorized.execution.enabled=false"**.
E com o comando **"set hive.vectorized.execution.enabled=true"** podemos ativá-la e conferir que agora a configuração está **ativada**.

```
0: jdbc:hive2://> set hive.vectorized.execution.enabled;
+------------------------------------------+--+
|                   set                    |
+------------------------------------------+--+
| hive.vectorized.execution.enabled=false  |
+------------------------------------------+--+
1 row selected (0.021 seconds)
0: jdbc:hive2://> set hive.vectorized.execution.enabled=true;
No rows affected (0.017 seconds)
0: jdbc:hive2://> set hive.vectorized.execution.enabled;
+-----------------------------------------+--+
|                   set                   |
+-----------------------------------------+--+
| hive.vectorized.execution.enabled=true  |
+-----------------------------------------+--+
1 row selected (0.013 seconds)
```

<br>
<br>

Em seguida faço a mesma query selecionando os mesmos dados da query anterior (sem a ativação da vetorização).
Agora podemos ver que ela levou cerca de **40.393 segundos**. Aqui, agora, com a vetorização **ativada**. 
Isso gerou uma diferença de mais de 30 segundos em um arquivo de 100 linhas, imagina em big data! Mas lembrando que isso é relativo (=

```
0: jdbc:hive2://> SELECT loc.datalocacao, loc.total, cli.nome FROM locacao_orc loc
. . . . . . . . > JOIN clientes_orc cli ON (loc.idcliente = cli.idcliente);
Query ID = cloudera_20230109112626_e0a8d08d-7617-4e52-a99f-fe44630af5d0
Total jobs = 1
Execution log at: /tmp/cloudera/cloudera_20230109112626_e0a8d08d-7617-4e52-a99f-fe44630af5d0.log
2023-01-09 11:26:54	Starting to launch local task to process map join;	maximum memory = 932184064
2023-01-09 11:26:57	Dump the side-table for tag: 0 with group count: 25 into file: file:/tmp/cloudera/e20f8781-074c-4a57-81e0-c9a6c48e9dbc/hive_2023-01-09_11-26-47_227_479619474645764209-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile10--.hashtable
2023-01-09 11:26:57	Uploaded 1 File to: file:/tmp/cloudera/e20f8781-074c-4a57-81e0-c9a6c48e9dbc/hive_2023-01-09_11-26-47_227_479619474645764209-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile10--.hashtable (2218 bytes)
2023-01-09 11:26:57	End of local task; Time Taken: 2.5 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks is set to 0 since there's no reduce operator
23/01/09 11:26:58 [HiveServer2-Background-Pool: Thread-99]: WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
Starting Job = job_1671629034454_0026, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1671629034454_0026/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1671629034454_0026
Execution log at: /tmp/cloudera/cloudera_20230109112626_e0a8d08d-7617-4e52-a99f-fe44630af5d0.log
2023-01-09 11:26:54	Starting to launch local task to process map join;	maximum memory = 932184064
2023-01-09 11:26:57	Dump the side-table for tag: 0 with group count: 25 into file: file:/tmp/cloudera/e20f8781-074c-4a57-81e0-c9a6c48e9dbc/hive_2023-01-09_11-26-47_227_479619474645764209-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile10--.hashtable
2023-01-09 11:26:57	Uploaded 1 File to: file:/tmp/cloudera/e20f8781-074c-4a57-81e0-c9a6c48e9dbc/hive_2023-01-09_11-26-47_227_479619474645764209-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile10--.hashtable (2218 bytes)
2023-01-09 11:26:57	End of local task; Time Taken: 2.5 sec.
Hadoop job information for Stage-3: number of mappers: 1; number of reducers: 0
23/01/09 11:27:08 [HiveServer2-Background-Pool: Thread-99]: WARN mapreduce.Counters: Group org.apache.hadoop.mapred.Task$Counter is deprecated. Use org.apache.hadoop.mapreduce.TaskCounter instead
2023-01-09 11:27:08,773 Stage-3 map = 0%,  reduce = 0%
2023-01-09 11:27:24,245 Stage-3 map = 100%,  reduce = 0%, Cumulative CPU 4.13 sec
MapReduce Total cumulative CPU time: 4 seconds 130 msec
Ended Job = job_1671629034454_0026
MapReduce Jobs Launched: 
Stage-Stage-3: Map: 1   Cumulative CPU: 4.13 sec   HDFS Read: 11325 HDFS Write: 3473 SUCCESS
Total MapReduce CPU Time Spent: 4 seconds 130 msec
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
100 rows selected (40.393 seconds)
```


<br>
<br>

Finalizo a descrição desse script aqui. Acrescento que são minhas observações durante meu aprendizado e que ainda estou no início, então peço paciência sobre qualquer erro (= Obrigada pela compreensão :smile:

Obrigada por ler até o final! :wink:




