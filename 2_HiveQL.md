# Explicando o desenvolvimento do Script "HiveQL" :pencil:

Esse foi o script onde mostro um pouco do desenvolvimento das querys dentro do Hive com as tabelas que criei no script 1.

<br>

> Podemos identificar como os comandos que inseri aquelas instruções que estão a frente do indicador "0: jdbc:hive2://> ".

<br>

Abaixo podemos ver a seleção de colunas específicas da tabela locacao com um join com todas as outras tabelas.
Durante a execução podemos ver o programa carregando os dados das colunas, os stages e o MapReduce agindo. No final indica as linhas e o tempo de execução.

```
0: jdbc:hive2://> SELECT cli.nome, des.nome, veic.modelo, loc.datalocacao, loc.total
. . . . . . . . > FROM locacao loc
. . . . . . . . > JOIN despachantes des ON (loc.iddespachante = des.iddespachante)
. . . . . . . . > JOIN clientes cli ON (loc.idcliente = cli.idcliente)
. . . . . . . . > JOIN veiculos veic ON (loc.idveiculo = veic.idveiculo);
22/12/22 09:14:13 [main]: ERROR calcite.RelOptHiveTable: No Stats for locacao@despachantes, Columns: iddespachante
22/12/22 09:14:13 [main]: ERROR parse.CalcitePlanner: CBO failed due to missing column stats (see previous errors), skipping CBO
Query ID = cloudera_20221222091414_6ea804db-c427-411c-b0d9-adcc722a5187
Total jobs = 1
Execution log at: /tmp/cloudera/cloudera_20221222091414_6ea804db-c427-411c-b0d9-adcc722a5187.log
2022-12-22 09:14:20	Starting to launch local task to process map join;	maximum memory = 932184064
2022-12-22 09:14:23	Dump the side-table for tag: 1 with group count: 30 into file: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-14-12_623_1371937727471897589-1/-local-10006/HashTable-Stage-7/MapJoin-mapfile31--.hashtable
2022-12-22 09:14:23	Uploaded 1 File to: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-14-12_623_1371937727471897589-1/-local-10006/HashTable-Stage-7/MapJoin-mapfile31--.hashtable (1458 bytes)
2022-12-22 09:14:23	Dump the side-table for tag: 1 with group count: 25 into file: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-14-12_623_1371937727471897589-1/-local-10006/HashTable-Stage-7/MapJoin-mapfile41--.hashtable
2022-12-22 09:14:23	Uploaded 1 File to: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-14-12_623_1371937727471897589-1/-local-10006/HashTable-Stage-7/MapJoin-mapfile41--.hashtable (1132 bytes)
2022-12-22 09:14:23	Dump the side-table for tag: 1 with group count: 10 into file: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-14-12_623_1371937727471897589-1/-local-10006/HashTable-Stage-7/MapJoin-mapfile51--.hashtable
2022-12-22 09:14:23	Uploaded 1 File to: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-14-12_623_1371937727471897589-1/-local-10006/HashTable-Stage-7/MapJoin-mapfile51--.hashtable (622 bytes)
2022-12-22 09:14:23	End of local task; Time Taken: 2.697 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks is set to 0 since there's no reduce operator
22/12/22 09:14:24 [HiveServer2-Background-Pool: Thread-435]: WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
Starting Job = job_1671629034454_0011, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1671629034454_0011/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1671629034454_0011
Execution log at: /tmp/cloudera/cloudera_20221222091414_6ea804db-c427-411c-b0d9-adcc722a5187.log
2022-12-22 09:14:20	Starting to launch local task to process map join;	maximum memory = 932184064
2022-12-22 09:14:23	Dump the side-table for tag: 1 with group count: 30 into file: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-14-12_623_1371937727471897589-1/-local-10006/HashTable-Stage-7/MapJoin-mapfile31--.hashtable
2022-12-22 09:14:23	Uploaded 1 File to: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-14-12_623_1371937727471897589-1/-local-10006/HashTable-Stage-7/MapJoin-mapfile31--.hashtable (1458 bytes)
2022-12-22 09:14:23	Dump the side-table for tag: 1 with group count: 25 into file: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-14-12_623_1371937727471897589-1/-local-10006/HashTable-Stage-7/MapJoin-mapfile41--.hashtable
2022-12-22 09:14:23	Uploaded 1 File to: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-14-12_623_1371937727471897589-1/-local-10006/HashTable-Stage-7/MapJoin-mapfile41--.hashtable (1132 bytes)
2022-12-22 09:14:23	Dump the side-table for tag: 1 with group count: 10 into file: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-14-12_623_1371937727471897589-1/-local-10006/HashTable-Stage-7/MapJoin-mapfile51--.hashtable
2022-12-22 09:14:23	Uploaded 1 File to: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-14-12_623_1371937727471897589-1/-local-10006/HashTable-Stage-7/MapJoin-mapfile51--.hashtable (622 bytes)
2022-12-22 09:14:23	End of local task; Time Taken: 2.697 sec.
Hadoop job information for Stage-7: number of mappers: 1; number of reducers: 0
22/12/22 09:14:35 [HiveServer2-Background-Pool: Thread-435]: WARN mapreduce.Counters: Group org.apache.hadoop.mapred.Task$Counter is deprecated. Use org.apache.hadoop.mapreduce.TaskCounter instead
2022-12-22 09:14:35,984 Stage-7 map = 0%,  reduce = 0%
2022-12-22 09:14:57,220 Stage-7 map = 100%,  reduce = 0%, Cumulative CPU 3.39 sec
MapReduce Total cumulative CPU time: 3 seconds 390 msec
Ended Job = job_1671629034454_0011
MapReduce Jobs Launched: 
Stage-Stage-7: Map: 1   Cumulative CPU: 3.39 sec   HDFS Read: 14118 HDFS Write: 7368 SUCCESS
Total MapReduce CPU Time Spent: 3 seconds 390 msec
OK
+---------------------+----------------------+------------------------------+------------------+------------+--+
|      cli.nome       |       des.nome       |         veic.modelo          | loc.datalocacao  | loc.total  |
+---------------------+----------------------+------------------------------+------------------+------------+--+
| Carminda Veríssimo  | Graça Ornellas       | Infiniti Q50S 3.7 Sedan      | 2019-02-20       | 1996.0     |
| Clóvis Carrasco     | Graça Ornellas       | BMW 750i xDrive              | 2019-06-30       | 1843.0     |
| Rebeca Torcuato     | Graça Ornellas       | BMW 750i xDrive              | 2019-03-09       | 2016.0     |
| Xisto Mendoça       | Emídio Dornelles     | Volvo XC90 T8 Hybrid         | 2019-04-19       | 1857.0     |
| Hipólito Granja     | Viviana Sequeira     | BMW 750i xDrive              | 2019-02-24       | 2049.0     |
| Márcio Taveira      | Graça Ornellas       | BMW 750i xDrive              | 2019-05-15       | 1986.0     |
| Micael Mangueira    | Deolinda Vilela      | Volvo XC90 T8 Hybrid         | 2019-05-01       | 1989.0     |
| Vanderlei Açores    | Graça Ornellas       | Infiniti Q50S 3.7 Sedan      | 2019-03-29       | 1874.0     |
| Rebeca Torcuato     | Noêmia   Orriça      | Mercedes-Benz S65 AMG Coupe  | 2019-05-10       | 2027.0     |
| Alvito Espargosa    | Deolinda Vilela      | Tesla Model S P90D           | 2019-03-17       | 2010.0     |
| Iuri Alancastre     | Graça Ornellas       | Mercedes-Benz S65 AMG Coupe  | 2019-05-12       | 2006.0     |
| Sabino Abrantes     | Viviana Sequeira     | Volvo XC90 T8 Hybrid         | 2019-02-26       | 1915.0     |
| Severino Leiria     | Matilde Rebouças     | BMW 750i xDrive              | 2019-06-12       | 2089.0     |
| Xisto Mendoça       | Graça Ornellas       | BMW 750i xDrive              | 2019-06-04       | 1942.0     |
| Isaura Farias       | Viviana Sequeira     | Tesla Model S P90D           | 2019-02-22       | 2071.0     |
| Rebeca Torcuato     | Felisbela Dornelles  | Mercedes-Benz S65 AMG Coupe  | 2019-05-29       | 2035.0     |
| Cleusa Lamego       | Carminda Pestana     | Infiniti Q50S 3.7 Sedan      | 2019-04-09       | 1810.0     |
| Iara Cardoso        | Roque Vásquez        | Infiniti Q50S 3.7 Sedan      | 2019-04-21       | 2017.0     |
| Carminda Veríssimo  | Noêmia   Orriça      | Infiniti Q50S 3.7 Sedan      | 2019-05-11       | 2095.0     |
| Alvito Espargosa    | Viviana Sequeira     | Volvo XC90 T8 Hybrid         | 2019-06-17       | 2020.0     |
| Micael Mangueira    | Matilde Rebouças     | Tesla Model S P90D           | 2019-04-12       | 2077.0     |
| Paula Padilha       | Matilde Rebouças     | Infiniti Q50S 3.7 Sedan      | 2019-06-02       | 1867.0     |
| Sabino Abrantes     | Uriel Queiroz        | Volvo XC90 T8 Hybrid         | 2019-05-12       | 1862.0     |
| Micael Mangueira    | Matilde Rebouças     | Volvo XC90 T8 Hybrid         | 2019-04-06       | 1855.0     |
| Tobias Garcés       | Felisbela Dornelles  | Tesla Model S P90D           | 2019-04-26       | 2033.0     |
| Zeferino Matoso     | Viviana Sequeira     | Infiniti Q50S 3.7 Sedan      | 2019-03-16       | 2034.0     |
| Hipólito Granja     | Deolinda Vilela      | BMW 750i xDrive              | 2019-05-06       | 1865.0     |
| Noêmia   Tupinambá  | Emídio Dornelles     | Tesla Model S P90D           | 2019-06-05       | 1997.0     |
| Vanderlei Açores    | Noêmia   Orriça      | Infiniti Q50S 3.7 Sedan      | 2019-05-07       | 2032.0     |
| Micael Mangueira    | Emídio Dornelles     | BMW 750i xDrive              | 2019-05-06       | 2011.0     |
| Márcio Taveira      | Carminda Pestana     | Tesla Model S P90D           | 2019-03-26       | 1867.0     |
| Sabino Abrantes     | Uriel Queiroz        | Mercedes-Benz S65 AMG Coupe  | 2019-03-14       | 1963.0     |
| Cátia Fróis         | Carminda Pestana     | Volvo XC90 T8 Hybrid         | 2019-04-15       | 1845.0     |
| Micael Mangueira    | Deolinda Vilela      | BMW 750i xDrive              | 2019-06-28       | 2020.0     |
| Noêmia   Tupinambá  | Felisbela Dornelles  | Volvo XC90 T8 Hybrid         | 2019-06-22       | 1938.0     |
| Clóvis Carrasco     | Carminda Pestana     | BMW 750i xDrive              | 2019-06-01       | 1980.0     |
| Iuri Alancastre     | Felisbela Dornelles  | Volvo XC90 T8 Hybrid         | 2019-03-21       | 2037.0     |
| Xisto Mendoça       | Matilde Rebouças     | BMW 750i xDrive              | 2019-05-31       | 1970.0     |
| Emílio Faro         | Viviana Sequeira     | BMW 750i xDrive              | 2019-05-02       | 1900.0     |
| Tobias Garcés       | Carminda Pestana     | BMW 750i xDrive              | 2019-06-15       | 1888.0     |
| Rebeca Torcuato     | Viviana Sequeira     | Mercedes-Benz S65 AMG Coupe  | 2019-02-20       | 2042.0     |
| Iara Cardoso        | Carminda Pestana     | Infiniti Q50S 3.7 Sedan      | 2019-03-04       | 1821.0     |
| Cleusa Lamego       | Emídio Dornelles     | Tesla Model S P90D           | 2019-05-21       | 2009.0     |
| Severino Leiria     | Carminda Pestana     | Infiniti Q50S 3.7 Sedan      | 2019-04-02       | 1837.0     |
| Micael Mangueira    | Uriel Queiroz        | Tesla Model S P90D           | 2019-06-08       | 1971.0     |
| Cátia Fróis         | Emídio Dornelles     | Tesla Model S P90D           | 2019-05-28       | 1818.0     |
| Noêmia   Tupinambá  | Graça Ornellas       | Volvo XC90 T8 Hybrid         | 2019-02-22       | 2031.0     |
| Iara Cardoso        | Noêmia   Orriça      | Infiniti Q50S 3.7 Sedan      | 2019-06-04       | 1938.0     |
| Alvito Espargosa    | Deolinda Vilela      | Infiniti Q50S 3.7 Sedan      | 2019-03-08       | 1975.0     |
| Vicente Rosa        | Viviana Sequeira     | Tesla Model S P90D           | 2019-02-27       | 2014.0     |
| Rebeca Torcuato     | Viviana Sequeira     | Volvo XC90 T8 Hybrid         | 2019-03-15       | 1958.0     |
| Tobias Garcés       | Viviana Sequeira     | Tesla Model S P90D           | 2019-05-11       | 2024.0     |
| Laura Marcondes     | Carminda Pestana     | Volvo XC90 T8 Hybrid         | 2019-04-20       | 1946.0     |
| Vicente Rosa        | Matilde Rebouças     | Infiniti Q50S 3.7 Sedan      | 2019-05-07       | 1855.0     |
| Micael Mangueira    | Graça Ornellas       | Volvo XC90 T8 Hybrid         | 2019-03-05       | 2039.0     |
| Paula Padilha       | Emídio Dornelles     | BMW 750i xDrive              | 2019-03-18       | 1991.0     |
| Carminda Veríssimo  | Graça Ornellas       | Infiniti Q50S 3.7 Sedan      | 2019-03-18       | 1914.0     |
| Emílio Faro         | Graça Ornellas       | BMW 750i xDrive              | 2019-04-21       | 1890.0     |
| Vicente Rosa        | Noêmia   Orriça      | Volvo XC90 T8 Hybrid         | 2019-06-21       | 1840.0     |
| Noêmia   Tupinambá  | Carminda Pestana     | Mercedes-Benz S65 AMG Coupe  | 2019-03-27       | 2004.0     |
| Iara Cardoso        | Deolinda Vilela      | Volvo XC90 T8 Hybrid         | 2019-03-08       | 2075.0     |
| Vicente Rosa        | Graça Ornellas       | Volvo XC90 T8 Hybrid         | 2019-05-10       | 1838.0     |
| Clóvis Carrasco     | Uriel Queiroz        | BMW 750i xDrive              | 2019-05-21       | 1974.0     |
| Vicente Rosa        | Felisbela Dornelles  | Infiniti Q50S 3.7 Sedan      | 2019-02-26       | 2064.0     |
| Vanderlei Açores    | Roque Vásquez        | BMW 750i xDrive              | 2019-05-22       | 1836.0     |
| Iuri Alancastre     | Felisbela Dornelles  | Volvo XC90 T8 Hybrid         | 2019-04-20       | 1800.0     |
| Blasco Canto        | Noêmia   Orriça      | BMW 750i xDrive              | 2019-06-22       | 2070.0     |
| Cleusa Lamego       | Graça Ornellas       | BMW 750i xDrive              | 2019-04-11       | 2001.0     |
| Iuri Alancastre     | Matilde Rebouças     | Volvo XC90 T8 Hybrid         | 2019-02-20       | 1858.0     |
| Paula Padilha       | Felisbela Dornelles  | Infiniti Q50S 3.7 Sedan      | 2019-05-26       | 1983.0     |
| Cátia Fróis         | Carminda Pestana     | BMW 750i xDrive              | 2019-06-08       | 1853.0     |
| Emílio Faro         | Deolinda Vilela      | Mercedes-Benz S65 AMG Coupe  | 2019-06-14       | 1813.0     |
| Iuri Alancastre     | Noêmia   Orriça      | Mercedes-Benz S65 AMG Coupe  | 2019-03-27       | 1950.0     |
| Cátia Fróis         | Graça Ornellas       | Volvo XC90 T8 Hybrid         | 2019-06-23       | 1831.0     |
| Severino Leiria     | Uriel Queiroz        | Mercedes-Benz S65 AMG Coupe  | 2019-06-03       | 2049.0     |
| Emílio Faro         | Roque Vásquez        | BMW 750i xDrive              | 2019-05-13       | 1843.0     |
| Xisto Mendoça       | Carminda Pestana     | BMW 750i xDrive              | 2019-06-26       | 1881.0     |
| Rosana Betancour    | Graça Ornellas       | Mercedes-Benz S65 AMG Coupe  | 2019-05-19       | 1810.0     |
| Emílio Faro         | Noêmia   Orriça      | Infiniti Q50S 3.7 Sedan      | 2019-04-28       | 1899.0     |
| Paula Padilha       | Carminda Pestana     | BMW 750i xDrive              | 2019-06-15       | 1812.0     |
| Vanderlei Açores    | Deolinda Vilela      | Infiniti Q50S 3.7 Sedan      | 2019-04-08       | 1921.0     |
| Iara Cardoso        | Matilde Rebouças     | Volvo XC90 T8 Hybrid         | 2019-04-03       | 1945.0     |
| Iuri Alancastre     | Matilde Rebouças     | Volvo XC90 T8 Hybrid         | 2019-04-01       | 1990.0     |
| Cátia Fróis         | Roque Vásquez        | Mercedes-Benz S65 AMG Coupe  | 2019-04-16       | 1956.0     |
| Vicente Rosa        | Viviana Sequeira     | Volvo XC90 T8 Hybrid         | 2019-04-05       | 1927.0     |
| Vicente Rosa        | Matilde Rebouças     | Mercedes-Benz S65 AMG Coupe  | 2019-04-23       | 1801.0     |
| Iuri Alancastre     | Matilde Rebouças     | BMW 750i xDrive              | 2019-03-22       | 1802.0     |
| Iara Cardoso        | Graça Ornellas       | Volvo XC90 T8 Hybrid         | 2019-02-27       | 1851.0     |
| Paula Padilha       | Felisbela Dornelles  | Volvo XC90 T8 Hybrid         | 2019-06-22       | 2089.0     |
| Emílio Faro         | Graça Ornellas       | Mercedes-Benz S65 AMG Coupe  | 2019-03-27       | 1967.0     |
| Tobias Garcés       | Carminda Pestana     | BMW 750i xDrive              | 2019-05-07       | 1826.0     |
| Xisto Mendoça       | Uriel Queiroz        | BMW 750i xDrive              | 2019-05-13       | 1864.0     |
| Micael Mangueira    | Graça Ornellas       | BMW 750i xDrive              | 2019-03-10       | 2056.0     |
| Rosana Betancour    | Matilde Rebouças     | Mercedes-Benz S65 AMG Coupe  | 2019-05-22       | 1986.0     |
| Vanderlei Açores    | Deolinda Vilela      | BMW 750i xDrive              | 2019-02-21       | 1861.0     |
| Clóvis Carrasco     | Viviana Sequeira     | Volvo XC90 T8 Hybrid         | 2019-06-22       | 1860.0     |
| Paula Padilha       | Uriel Queiroz        | Tesla Model S P90D           | 2019-02-27       | 2100.0     |
| Hipólito Granja     | Emídio Dornelles     | Volvo XC90 T8 Hybrid         | 2019-05-29       | 2054.0     |
| Alvito Espargosa    | Emídio Dornelles     | BMW 750i xDrive              | 2019-03-30       | 1886.0     |
| Noêmia   Tupinambá  | Roque Vásquez        | Volvo XC90 T8 Hybrid         | 2019-06-12       | 1939.0     |
+---------------------+----------------------+------------------------------+------------------+------------+--+
100 rows selected (48.451 seconds)
```

<br>

Abaixo podemos ver a seleção da soma dos totais de locação por modelo de veículo da tabela locacao com um join com a tabela veiculos, tudo isso agrupado por modelo.
Durante a execução podemos ver o programa carregando os dados das colunas, os stages e o MapReduce agindo. No final indica as linhas e o tempo de execução.

```
: jdbc:hive2://> SELECT veic.modelo, SUM(loc.total)
. . . . . . . . > FROM locacao loc
. . . . . . . . > JOIN veiculos veic ON (loc.idveiculo = veic.idveiculo)
. . . . . . . . > GROUP BY veic.modelo;
22/12/22 09:27:36 [main]: WARN parse.RowResolver: Duplicate column info for veic.modelo was overwritten in RowResolver map: _col0: string by _col0: string
Query ID = cloudera_20221222092727_4e40bc26-b993-4621-afad-0bc995446dae
Total jobs = 1
Execution log at: /tmp/cloudera/cloudera_20221222092727_4e40bc26-b993-4621-afad-0bc995446dae.log
2022-12-22 09:27:44	Starting to launch local task to process map join;	maximum memory = 932184064
2022-12-22 09:27:47	Dump the side-table for tag: 1 with group count: 30 into file: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-27-36_646_6499238547507786282-1/-local-10004/HashTable-Stage-2/MapJoin-mapfile61--.hashtable
2022-12-22 09:27:47	Uploaded 1 File to: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-27-36_646_6499238547507786282-1/-local-10004/HashTable-Stage-2/MapJoin-mapfile61--.hashtable (1458 bytes)
2022-12-22 09:27:47	End of local task; Time Taken: 3.682 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
22/12/22 09:27:49 [HiveServer2-Background-Pool: Thread-464]: WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
Starting Job = job_1671629034454_0012, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1671629034454_0012/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1671629034454_0012
Hadoop job information for Stage-2: number of mappers: 1; number of reducers: 1
22/12/22 09:28:00 [HiveServer2-Background-Pool: Thread-464]: WARN mapreduce.Counters: Group org.apache.hadoop.mapred.Task$Counter is deprecated. Use org.apache.hadoop.mapreduce.TaskCounter instead
2022-12-22 09:28:00,741 Stage-2 map = 0%,  reduce = 0%
Execution log at: /tmp/cloudera/cloudera_20221222092727_4e40bc26-b993-4621-afad-0bc995446dae.log
2022-12-22 09:27:44	Starting to launch local task to process map join;	maximum memory = 932184064
2022-12-22 09:27:47	Dump the side-table for tag: 1 with group count: 30 into file: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-27-36_646_6499238547507786282-1/-local-10004/HashTable-Stage-2/MapJoin-mapfile61--.hashtable
2022-12-22 09:27:47	Uploaded 1 File to: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-27-36_646_6499238547507786282-1/-local-10004/HashTable-Stage-2/MapJoin-mapfile61--.hashtable (1458 bytes)
2022-12-22 09:27:47	End of local task; Time Taken: 3.682 sec.
2022-12-22 09:28:27,928 Stage-2 map = 100%,  reduce = 0%, Cumulative CPU 4.13 sec
2022-12-22 09:28:43,317 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 5.87 sec
MapReduce Total cumulative CPU time: 5 seconds 870 msec
Ended Job = job_1671629034454_0012
MapReduce Jobs Launched: 
Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 5.87 sec   HDFS Read: 14885 HDFS Write: 148 SUCCESS
Total MapReduce CPU Time Spent: 5 seconds 870 msec
OK
+------------------------------+----------+--+
|         veic.modelo          |   _c1    |
+------------------------------+----------+--+
| BMW 750i xDrive              | 56005.0  |
| Infiniti Q50S 3.7 Sedan      | 34932.0  | 
| Mercedes-Benz S65 AMG Coupe  | 27409.0  |
| Tesla Model S P90D           | 23991.0  |
| Volvo XC90 T8 Hybrid         | 52189.0  |
+------------------------------+----------+--+
5 rows selected (70.26 seconds)
```

<br>

Abaixo podemos ver a seleção da soma dos totais de locação (somente para totais acima de 10000 ) por modelo de veículo e por despachante da tabela locacao , com  join com a tabela veiculos e despachantes, tudo isso agrupado por modelo e nome do despachante.
Durante a execução podemos ver o programa carregando os dados das colunas, os stages e o MapReduce agindo. No final indica as linhas e o tempo de execução.

```
0: jdbc:hive2://> SELECT veic.modelo, desp.nome, SUM(loc.total)
. . . . . . . . > FROM locacao loc
. . . . . . . . > JOIN veiculos veic ON (loc.idveiculo = veic.idveiculo)
. . . . . . . . > JOIN despachantes desp ON (loc.iddespachante = desp.iddespachante)
. . . . . . . . > GROUP BY veic.modelo, desp.nome
. . . . . . . . > HAVING SUM(loc.total) > 10000;
22/12/22 09:39:09 [main]: WARN parse.RowResolver: Duplicate column info for veic.modelo was overwritten in RowResolver map: _col0: string by _col0: string
22/12/22 09:39:09 [main]: WARN parse.RowResolver: Duplicate column info for desp.nome was overwritten in RowResolver map: _col1: string by _col1: string
22/12/22 09:39:09 [main]: ERROR calcite.RelOptHiveTable: No Stats for locacao@veiculos, Columns: idveiculo
22/12/22 09:39:09 [main]: ERROR parse.CalcitePlanner: CBO failed due to missing column stats (see previous errors), skipping CBO
22/12/22 09:39:10 [main]: WARN parse.RowResolver: Duplicate column info for veic.modelo was overwritten in RowResolver map: _col0: string by _col0: string
22/12/22 09:39:10 [main]: WARN parse.RowResolver: Duplicate column info for desp.nome was overwritten in RowResolver map: _col1: string by _col1: string
Query ID = cloudera_20221222093939_a6e0c57f-1a34-47d2-a83a-a32277feac80
Total jobs = 1
Execution log at: /tmp/cloudera/cloudera_20221222093939_a6e0c57f-1a34-47d2-a83a-a32277feac80.log
2022-12-22 09:39:16	Starting to launch local task to process map join;	maximum memory = 932184064
2022-12-22 09:39:18	Dump the side-table for tag: 1 with group count: 10 into file: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-39-08_745_1607586083261458773-1/-local-10006/HashTable-Stage-3/MapJoin-mapfile71--.hashtable
2022-12-22 09:39:19	Uploaded 1 File to: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-39-08_745_1607586083261458773-1/-local-10006/HashTable-Stage-3/MapJoin-mapfile71--.hashtable (622 bytes)
2022-12-22 09:39:19	Dump the side-table for tag: 1 with group count: 30 into file: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-39-08_745_1607586083261458773-1/-local-10006/HashTable-Stage-3/MapJoin-mapfile81--.hashtable
2022-12-22 09:39:19	Uploaded 1 File to: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-39-08_745_1607586083261458773-1/-local-10006/HashTable-Stage-3/MapJoin-mapfile81--.hashtable (1458 bytes)
2022-12-22 09:39:19	End of local task; Time Taken: 2.479 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
22/12/22 09:39:20 [HiveServer2-Background-Pool: Thread-497]: WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
Starting Job = job_1671629034454_0013, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1671629034454_0013/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1671629034454_0013
Execution log at: /tmp/cloudera/cloudera_20221222093939_a6e0c57f-1a34-47d2-a83a-a32277feac80.log
2022-12-22 09:39:16	Starting to launch local task to process map join;	maximum memory = 932184064
2022-12-22 09:39:18	Dump the side-table for tag: 1 with group count: 10 into file: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-39-08_745_1607586083261458773-1/-local-10006/HashTable-Stage-3/MapJoin-mapfile71--.hashtable
2022-12-22 09:39:19	Uploaded 1 File to: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-39-08_745_1607586083261458773-1/-local-10006/HashTable-Stage-3/MapJoin-mapfile71--.hashtable (622 bytes)
2022-12-22 09:39:19	Dump the side-table for tag: 1 with group count: 30 into file: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-39-08_745_1607586083261458773-1/-local-10006/HashTable-Stage-3/MapJoin-mapfile81--.hashtable
2022-12-22 09:39:19	Uploaded 1 File to: file:/tmp/cloudera/9a6b728d-fac5-4d6c-bed0-b92269029feb/hive_2022-12-22_09-39-08_745_1607586083261458773-1/-local-10006/HashTable-Stage-3/MapJoin-mapfile81--.hashtable (1458 bytes)
2022-12-22 09:39:19	End of local task; Time Taken: 2.479 sec.
Hadoop job information for Stage-3: number of mappers: 1; number of reducers: 1
22/12/22 09:39:33 [HiveServer2-Background-Pool: Thread-497]: WARN mapreduce.Counters: Group org.apache.hadoop.mapred.Task$Counter is deprecated. Use org.apache.hadoop.mapreduce.TaskCounter instead
2022-12-22 09:39:33,468 Stage-3 map = 0%,  reduce = 0%
2022-12-22 09:39:50,294 Stage-3 map = 100%,  reduce = 0%, Cumulative CPU 4.32 sec
2022-12-22 09:40:03,124 Stage-3 map = 100%,  reduce = 100%, Cumulative CPU 6.75 sec
MapReduce Total cumulative CPU time: 6 seconds 750 msec
Ended Job = job_1671629034454_0013
MapReduce Jobs Launched: 
Stage-Stage-3: Map: 1  Reduce: 1   Cumulative CPU: 6.75 sec   HDFS Read: 17709 HDFS Write: 83 SUCCESS
Total MapReduce CPU Time Spent: 6 seconds 750 msec
OK
+------------------+-------------------+----------+--+
|   veic.modelo    |     desp.nome     |   _c2    |
+------------------+-------------------+----------+--+
| BMW 750i xDrive  | Carminda Pestana  | 11240.0  |
| BMW 750i xDrive  | Graça Ornellas    | 13734.0  |
+------------------+-------------------+----------+--+
2 rows selected (58.628 seconds)
```

<br>
<br>

Finalizo a descrição desse script aqui. Acrescento que são minhas observações durante meu aprendizado e que ainda estou no início, então peço paciência sobre qualquer erro (= Obrigada pela compreensão :smile:

Obrigada por ler até o final! :wink:




