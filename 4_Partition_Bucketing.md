# Explicando o desenvolvimento do Script "Partition_Bucketing" :pencil:

Esse é o script onde mostro um pouco do desenvolvimento do processo de particionamento e bucketing.

<br>

![image](https://user-images.githubusercontent.com/102270053/213726778-58f1408b-515c-4ed8-b23b-66206ef68d72.png)

<br>

## Overview Partition e Bucketing

De início, é importante ressaltar que, tanto hive partition quanto hive bucketing, são técnicoas de otimização de performance de armazenamento e posterior processamento e acesso.

O **particionamento** é, cruamente falando, a divisão das tabelas em partições colunares, lógicas e físicas, em sub-diretórios, com o objetivo de otimizar as consultas com querys mais específicas e otiizadas. Mas vale pontuar a atenção a possibilidade de milhares ou milhões de partições, justamente por variar de acordo com os dados.

Já o **bucketing** é o conceito de dividir os dados em intervalos para dar "estrutura extra" aos dados. Este intervalo é determinado pelo valor de hash de uma ou mais colunas no conjunto de dados. Ele divide de forma balanceada os dados em partições limitadas e indicadas pelo usuário.

<br>
<br>

## Analise do código

<br>

> Podemos identificar como os comandos que inseri aquelas instruções que estão a frente do indicador "0: jdbc:hive2://> ".

<br>

Primeiramente, já logada no Hive, eu confiro a configuração do hive **"hive.exec.dynamic.partition.mode"** que, por padrão, está "desativada". 
Para que se torne possivel a criação de tabelas com armazenamento e processamento particionado, eu ativo essa mesma configuração.

```
0: jdbc:hive2://> set hive.exec.dynamic.partition.mode;
+------------------------------------------+--+
|                   set                    |
+------------------------------------------+--+
| hive.exec.dynamic.partition.mode=strict  |
+------------------------------------------+--+
1 row selected (0.178 seconds)
0: jdbc:hive2://> set hive.exec.dynamic.partition.mode=nonstrict;
No rows affected (0.375 seconds)
0: jdbc:hive2://> set hive.exec.dynamic.partition.mode;
+---------------------------------------------+--+
|                     set                     |
+---------------------------------------------+--+
| hive.exec.dynamic.partition.mode=nonstrict  |
+---------------------------------------------+--+
1 row selected (0.01 seconds)
```

<br>
<br>

Já com a configuração ativada consigo criar a tabela "locacaoanalitico" com a inserção do comando de particionamento **"PARTITIONED BY (veiculo string);"**, e popular a mesma.

```
0: jdbc:hive2://> create table locacaoanalitico (cliente string, despachante string, datalocacao date, total double)
. . . . . . . . > PARTITIONED BY (veiculo string);
OK
No rows affected (6.632 seconds)
0: jdbc:hive2://> INSERT OVERWRITE TABLE locacaoanalitico PARTITION (veiculo)
. . . . . . . . > SELECT cli.nome, des.nome, loc.datalocacao, loc.total, veic.modelo
. . . . . . . . > FROM locacao loc
. . . . . . . . > JOIN despachantes des ON (loc.iddespachante = des.iddespachante)
. . . . . . . . > JOIN clientes cli ON (loc.idcliente = cli.idcliente)
. . . . . . . . > JOIN veiculos veic ON (loc.idveiculo = veic.idveiculo);
23/01/05 07:09:51 [main]: WARN parse.BaseSemanticAnalyzer: Dynamic partitioning is used; only validating 0 columns
23/01/05 07:09:55 [main]: ERROR calcite.RelOptHiveTable: No Stats for locacao@despachantes, Columns: iddespachante
23/01/05 07:09:55 [main]: ERROR parse.CalcitePlanner: CBO failed due to missing column stats (see previous errors), skipping CBO
23/01/05 07:09:55 [main]: WARN parse.BaseSemanticAnalyzer: Dynamic partitioning is used; only validating 0 columns
Query ID = cloudera_20230105070909_a2208360-7952-45a8-95f6-73edd1ce1271
Total jobs = 1
Execution log at: /tmp/cloudera/cloudera_20230105070909_a2208360-7952-45a8-95f6-73edd1ce1271.log
2023-01-05 07:10:17	Starting to launch local task to process map join;	maximum memory = 932184064
Execution log at: /tmp/cloudera/cloudera_20230105070909_a2208360-7952-45a8-95f6-73edd1ce1271.log
2023-01-05 07:10:17	Starting to launch local task to process map join;	maximum memory = 932184064
2023-01-05 07:10:25	Dump the side-table for tag: 1 with group count: 30 into file: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_07-09-48_471_3492975002195570324-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile01--.hashtable
2023-01-05 07:10:26	Uploaded 1 File to: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_07-09-48_471_3492975002195570324-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile01--.hashtable (1458 bytes)
2023-01-05 07:10:26	Dump the side-table for tag: 1 with group count: 25 into file: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_07-09-48_471_3492975002195570324-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile11--.hashtable
2023-01-05 07:10:26	Uploaded 1 File to: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_07-09-48_471_3492975002195570324-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile11--.hashtable (1132 bytes)
2023-01-05 07:10:26	Dump the side-table for tag: 1 with group count: 10 into file: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_07-09-48_471_3492975002195570324-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile21--.hashtable
2023-01-05 07:10:26	Uploaded 1 File to: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_07-09-48_471_3492975002195570324-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile21--.hashtable (622 bytes)
2023-01-05 07:10:26	End of local task; Time Taken: 8.486 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks is set to 0 since there's no reduce operator
2023-01-05 07:10:25	Dump the side-table for tag: 1 with group count: 30 into file: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_07-09-48_471_3492975002195570324-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile01--.hashtable
2023-01-05 07:10:26	Uploaded 1 File to: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_07-09-48_471_3492975002195570324-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile01--.hashtable (1458 bytes)
2023-01-05 07:10:26	Dump the side-table for tag: 1 with group count: 25 into file: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_07-09-48_471_3492975002195570324-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile11--.hashtable
2023-01-05 07:10:26	Uploaded 1 File to: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_07-09-48_471_3492975002195570324-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile11--.hashtable (1132 bytes)
2023-01-05 07:10:26	Dump the side-table for tag: 1 with group count: 10 into file: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_07-09-48_471_3492975002195570324-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile21--.hashtable
2023-01-05 07:10:26	Uploaded 1 File to: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_07-09-48_471_3492975002195570324-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile21--.hashtable (622 bytes)
2023-01-05 07:10:26	End of local task; Time Taken: 8.486 sec.
23/01/05 07:10:30 [HiveServer2-Background-Pool: Thread-38]: WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
Starting Job = job_1671629034454_0021, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1671629034454_0021/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1671629034454_0021
Hadoop job information for Stage-8: number of mappers: 1; number of reducers: 0
23/01/05 07:10:54 [HiveServer2-Background-Pool: Thread-38]: WARN mapreduce.Counters: Group org.apache.hadoop.mapred.Task$Counter is deprecated. Use org.apache.hadoop.mapreduce.TaskCounter instead 
2023-01-05 07:10:54,040 Stage-8 map = 0%,  reduce = 0%
2023-01-05 07:11:28,545 Stage-8 map = 100%,  reduce = 0%, Cumulative CPU 7.54 sec
MapReduce Total cumulative CPU time: 7 seconds 540 msec
Ended Job = job_1671629034454_0021
Loading data to table locacao.locacaoanalitico partition (veiculo=null)
	 Time taken for load dynamic partitions : 4727
	Loading partition {veiculo=Volvo XC90 T8 Hybrid}
	Loading partition {veiculo=Mercedes-Benz S65 AMG Coupe}
	Loading partition {veiculo=Infiniti Q50S 3.7 Sedan}
	Loading partition {veiculo=BMW 750i xDrive}
	Loading partition {veiculo=Tesla Model S P90D}
	 Time taken for adding to write entity : 48
Partition locacao.locacaoanalitico{veiculo=BMW 750i xDrive} stats: [numFiles=1, numRows=29, totalSize=1453, rawDataSize=1424]
Partition locacao.locacaoanalitico{veiculo=Infiniti Q50S 3.7 Sedan} stats: [numFiles=1, numRows=18, totalSize=918, rawDataSize=900]
Partition locacao.locacaoanalitico{veiculo=Mercedes-Benz S65 AMG Coupe} stats: [numFiles=1, numRows=14, totalSize=705, rawDataSize=691]
Partition locacao.locacaoanalitico{veiculo=Tesla Model S P90D} stats: [numFiles=1, numRows=12, totalSize=607, rawDataSize=595]
Partition locacao.locacaoanalitico{veiculo=Volvo XC90 T8 Hybrid} stats: [numFiles=1, numRows=27, totalSize=1380, rawDataSize=1353]
MapReduce Jobs Launched: 
Stage-Stage-8: Map: 1   Cumulative CPU: 7.54 sec   HDFS Read: 14022 HDFS Write: 5470 SUCCESS
Total MapReduce CPU Time Spent: 7 seconds 540 msec
OK
No rows affected (114.487 seconds)
```

<br>
<br>

E ao fazermos o **"SELECT * FROM TABELAOANALITICO"** podemos ver que a execução do comando leva somente **0.869 segundos**.
Logo logo vamos comparar com o tempo de execução da seleção dos mesmos dados mas de uma tabela não particionada.

```
0: jdbc:hive2://> SELECT * FROM locacaoanalitico;
OK
+---------------------------+-------------------------------+-------------------------------+-------------------------+------------------------------+--+
| locacaoanalitico.cliente  | locacaoanalitico.despachante  | locacaoanalitico.datalocacao  | locacaoanalitico.total  |   locacaoanalitico.veiculo   |
+---------------------------+-------------------------------+-------------------------------+-------------------------+------------------------------+--+
| Clóvis Carrasco           | Graça Ornellas                | 2019-06-30                    | 1843.0                  | BMW 750i xDrive              |
| Rebeca Torcuato           | Graça Ornellas                | 2019-03-09                    | 2016.0                  | BMW 750i xDrive              |
| Hipólito Granja           | Viviana Sequeira              | 2019-02-24                    | 2049.0                  | BMW 750i xDrive              |
| Márcio Taveira            | Graça Ornellas                | 2019-05-15                    | 1986.0                  | BMW 750i xDrive              |
| Severino Leiria           | Matilde Rebouças              | 2019-06-12                    | 2089.0                  | BMW 750i xDrive              |
| Xisto Mendoça             | Graça Ornellas                | 2019-06-04                    | 1942.0                  | BMW 750i xDrive              |
| Hipólito Granja           | Deolinda Vilela               | 2019-05-06                    | 1865.0                  | BMW 750i xDrive              |
| Micael Mangueira          | Emídio Dornelles              | 2019-05-06                    | 2011.0                  | BMW 750i xDrive              |
| Micael Mangueira          | Deolinda Vilela               | 2019-06-28                    | 2020.0                  | BMW 750i xDrive              |
| Clóvis Carrasco           | Carminda Pestana              | 2019-06-01                    | 1980.0                  | BMW 750i xDrive              |
| Xisto Mendoça             | Matilde Rebouças              | 2019-05-31                    | 1970.0                  | BMW 750i xDrive              |
| Emílio Faro               | Viviana Sequeira              | 2019-05-02                    | 1900.0                  | BMW 750i xDrive              |
| Tobias Garcés             | Carminda Pestana              | 2019-06-15                    | 1888.0                  | BMW 750i xDrive              |
| Paula Padilha             | Emídio Dornelles              | 2019-03-18                    | 1991.0                  | BMW 750i xDrive              |
| Emílio Faro               | Graça Ornellas                | 2019-04-21                    | 1890.0                  | BMW 750i xDrive              |
| Clóvis Carrasco           | Uriel Queiroz                 | 2019-05-21                    | 1974.0                  | BMW 750i xDrive              |
| Vanderlei Açores          | Roque Vásquez                 | 2019-05-22                    | 1836.0                  | BMW 750i xDrive              |
| Blasco Canto              | Noêmia   Orriça               | 2019-06-22                    | 2070.0                  | BMW 750i xDrive              |
| Cleusa Lamego             | Graça Ornellas                | 2019-04-11                    | 2001.0                  | BMW 750i xDrive              |
| Cátia Fróis               | Carminda Pestana              | 2019-06-08                    | 1853.0                  | BMW 750i xDrive              |
| Emílio Faro               | Roque Vásquez                 | 2019-05-13                    | 1843.0                  | BMW 750i xDrive              |
| Xisto Mendoça             | Carminda Pestana              | 2019-06-26                    | 1881.0                  | BMW 750i xDrive              |
| Paula Padilha             | Carminda Pestana              | 2019-06-15                    | 1812.0                  | BMW 750i xDrive              |
| Iuri Alancastre           | Matilde Rebouças              | 2019-03-22                    | 1802.0                  | BMW 750i xDrive              |
| Tobias Garcés             | Carminda Pestana              | 2019-05-07                    | 1826.0                  | BMW 750i xDrive              |
| Xisto Mendoça             | Uriel Queiroz                 | 2019-05-13                    | 1864.0                  | BMW 750i xDrive              |
| Micael Mangueira          | Graça Ornellas                | 2019-03-10                    | 2056.0                  | BMW 750i xDrive              |
| Vanderlei Açores          | Deolinda Vilela               | 2019-02-21                    | 1861.0                  | BMW 750i xDrive              |
| Alvito Espargosa          | Emídio Dornelles              | 2019-03-30                    | 1886.0                  | BMW 750i xDrive              |
| Carminda Veríssimo        | Graça Ornellas                | 2019-02-20                    | 1996.0                  | Infiniti Q50S 3.7 Sedan      |
| Vanderlei Açores          | Graça Ornellas                | 2019-03-29                    | 1874.0                  | Infiniti Q50S 3.7 Sedan      |
| Cleusa Lamego             | Carminda Pestana              | 2019-04-09                    | 1810.0                  | Infiniti Q50S 3.7 Sedan      |
| Iara Cardoso              | Roque Vásquez                 | 2019-04-21                    | 2017.0                  | Infiniti Q50S 3.7 Sedan      |
| Carminda Veríssimo        | Noêmia   Orriça               | 2019-05-11                    | 2095.0                  | Infiniti Q50S 3.7 Sedan      |
| Paula Padilha             | Matilde Rebouças              | 2019-06-02                    | 1867.0                  | Infiniti Q50S 3.7 Sedan      |
| Zeferino Matoso           | Viviana Sequeira              | 2019-03-16                    | 2034.0                  | Infiniti Q50S 3.7 Sedan      |
| Vanderlei Açores          | Noêmia   Orriça               | 2019-05-07                    | 2032.0                  | Infiniti Q50S 3.7 Sedan      |
| Iara Cardoso              | Carminda Pestana              | 2019-03-04                    | 1821.0                  | Infiniti Q50S 3.7 Sedan      |
| Severino Leiria           | Carminda Pestana              | 2019-04-02                    | 1837.0                  | Infiniti Q50S 3.7 Sedan      |
| Iara Cardoso              | Noêmia   Orriça               | 2019-06-04                    | 1938.0                  | Infiniti Q50S 3.7 Sedan      |
| Alvito Espargosa          | Deolinda Vilela               | 2019-03-08                    | 1975.0                  | Infiniti Q50S 3.7 Sedan      |
| Vicente Rosa              | Matilde Rebouças              | 2019-05-07                    | 1855.0                  | Infiniti Q50S 3.7 Sedan      |
| Carminda Veríssimo        | Graça Ornellas                | 2019-03-18                    | 1914.0                  | Infiniti Q50S 3.7 Sedan      |
| Vicente Rosa              | Felisbela Dornelles           | 2019-02-26                    | 2064.0                  | Infiniti Q50S 3.7 Sedan      |
| Paula Padilha             | Felisbela Dornelles           | 2019-05-26                    | 1983.0                  | Infiniti Q50S 3.7 Sedan      |
| Emílio Faro               | Noêmia   Orriça               | 2019-04-28                    | 1899.0                  | Infiniti Q50S 3.7 Sedan      |
| Vanderlei Açores          | Deolinda Vilela               | 2019-04-08                    | 1921.0                  | Infiniti Q50S 3.7 Sedan      |
| Rebeca Torcuato           | Noêmia   Orriça               | 2019-05-10                    | 2027.0                  | Mercedes-Benz S65 AMG Coupe  |
| Iuri Alancastre           | Graça Ornellas                | 2019-05-12                    | 2006.0                  | Mercedes-Benz S65 AMG Coupe  |
| Rebeca Torcuato           | Felisbela Dornelles           | 2019-05-29                    | 2035.0                  | Mercedes-Benz S65 AMG Coupe  |
| Sabino Abrantes           | Uriel Queiroz                 | 2019-03-14                    | 1963.0                  | Mercedes-Benz S65 AMG Coupe  |
| Rebeca Torcuato           | Viviana Sequeira              | 2019-02-20                    | 2042.0                  | Mercedes-Benz S65 AMG Coupe  |
| Noêmia   Tupinambá        | Carminda Pestana              | 2019-03-27                    | 2004.0                  | Mercedes-Benz S65 AMG Coupe  |
| Emílio Faro               | Deolinda Vilela               | 2019-06-14                    | 1813.0                  | Mercedes-Benz S65 AMG Coupe  |
| Iuri Alancastre           | Noêmia   Orriça               | 2019-03-27                    | 1950.0                  | Mercedes-Benz S65 AMG Coupe  |
| Severino Leiria           | Uriel Queiroz                 | 2019-06-03                    | 2049.0                  | Mercedes-Benz S65 AMG Coupe  |
| Rosana Betancour          | Graça Ornellas                | 2019-05-19                    | 1810.0                  | Mercedes-Benz S65 AMG Coupe  |
| Cátia Fróis               | Roque Vásquez                 | 2019-04-16                    | 1956.0                  | Mercedes-Benz S65 AMG Coupe  |
| Vicente Rosa              | Matilde Rebouças              | 2019-04-23                    | 1801.0                  | Mercedes-Benz S65 AMG Coupe  |
| Emílio Faro               | Graça Ornellas                | 2019-03-27                    | 1967.0                  | Mercedes-Benz S65 AMG Coupe  |
| Rosana Betancour          | Matilde Rebouças              | 2019-05-22                    | 1986.0                  | Mercedes-Benz S65 AMG Coupe  |
| Alvito Espargosa          | Deolinda Vilela               | 2019-03-17                    | 2010.0                  | Tesla Model S P90D           |
| Isaura Farias             | Viviana Sequeira              | 2019-02-22                    | 2071.0                  | Tesla Model S P90D           |
| Micael Mangueira          | Matilde Rebouças              | 2019-04-12                    | 2077.0                  | Tesla Model S P90D           |
| Tobias Garcés             | Felisbela Dornelles           | 2019-04-26                    | 2033.0                  | Tesla Model S P90D           |
| Noêmia   Tupinambá        | Emídio Dornelles              | 2019-06-05                    | 1997.0                  | Tesla Model S P90D           |
| Márcio Taveira            | Carminda Pestana              | 2019-03-26                    | 1867.0                  | Tesla Model S P90D           |
| Cleusa Lamego             | Emídio Dornelles              | 2019-05-21                    | 2009.0                  | Tesla Model S P90D           |
| Micael Mangueira          | Uriel Queiroz                 | 2019-06-08                    | 1971.0                  | Tesla Model S P90D           |
| Cátia Fróis               | Emídio Dornelles              | 2019-05-28                    | 1818.0                  | Tesla Model S P90D           |
| Vicente Rosa              | Viviana Sequeira              | 2019-02-27                    | 2014.0                  | Tesla Model S P90D           |
| Tobias Garcés             | Viviana Sequeira              | 2019-05-11                    | 2024.0                  | Tesla Model S P90D           |
| Paula Padilha             | Uriel Queiroz                 | 2019-02-27                    | 2100.0                  | Tesla Model S P90D           |
| Xisto Mendoça             | Emídio Dornelles              | 2019-04-19                    | 1857.0                  | Volvo XC90 T8 Hybrid         |
| Micael Mangueira          | Deolinda Vilela               | 2019-05-01                    | 1989.0                  | Volvo XC90 T8 Hybrid         |
| Sabino Abrantes           | Viviana Sequeira              | 2019-02-26                    | 1915.0                  | Volvo XC90 T8 Hybrid         |
| Alvito Espargosa          | Viviana Sequeira              | 2019-06-17                    | 2020.0                  | Volvo XC90 T8 Hybrid         |
| Sabino Abrantes           | Uriel Queiroz                 | 2019-05-12                    | 1862.0                  | Volvo XC90 T8 Hybrid         |
| Micael Mangueira          | Matilde Rebouças              | 2019-04-06                    | 1855.0                  | Volvo XC90 T8 Hybrid         |
| Cátia Fróis               | Carminda Pestana              | 2019-04-15                    | 1845.0                  | Volvo XC90 T8 Hybrid         |
| Noêmia   Tupinambá        | Felisbela Dornelles           | 2019-06-22                    | 1938.0                  | Volvo XC90 T8 Hybrid         |
| Iuri Alancastre           | Felisbela Dornelles           | 2019-03-21                    | 2037.0                  | Volvo XC90 T8 Hybrid         |
| Noêmia   Tupinambá        | Graça Ornellas                | 2019-02-22                    | 2031.0                  | Volvo XC90 T8 Hybrid         |
| Rebeca Torcuato           | Viviana Sequeira              | 2019-03-15                    | 1958.0                  | Volvo XC90 T8 Hybrid         |
| Laura Marcondes           | Carminda Pestana              | 2019-04-20                    | 1946.0                  | Volvo XC90 T8 Hybrid         |
| Micael Mangueira          | Graça Ornellas                | 2019-03-05                    | 2039.0                  | Volvo XC90 T8 Hybrid         |
| Vicente Rosa              | Noêmia   Orriça               | 2019-06-21                    | 1840.0                  | Volvo XC90 T8 Hybrid         |
| Iara Cardoso              | Deolinda Vilela               | 2019-03-08                    | 2075.0                  | Volvo XC90 T8 Hybrid         |
| Vicente Rosa              | Graça Ornellas                | 2019-05-10                    | 1838.0                  | Volvo XC90 T8 Hybrid         |
| Iuri Alancastre           | Felisbela Dornelles           | 2019-04-20                    | 1800.0                  | Volvo XC90 T8 Hybrid         |
| Iuri Alancastre           | Matilde Rebouças              | 2019-02-20                    | 1858.0                  | Volvo XC90 T8 Hybrid         |
| Cátia Fróis               | Graça Ornellas                | 2019-06-23                    | 1831.0                  | Volvo XC90 T8 Hybrid         |
| Iara Cardoso              | Matilde Rebouças              | 2019-04-03                    | 1945.0                  | Volvo XC90 T8 Hybrid         |
| Iuri Alancastre           | Matilde Rebouças              | 2019-04-01                    | 1990.0                  | Volvo XC90 T8 Hybrid         |
| Vicente Rosa              | Viviana Sequeira              | 2019-04-05                    | 1927.0                  | Volvo XC90 T8 Hybrid         |
| Iara Cardoso              | Graça Ornellas                | 2019-02-27                    | 1851.0                  | Volvo XC90 T8 Hybrid         |
| Paula Padilha             | Felisbela Dornelles           | 2019-06-22                    | 2089.0                  | Volvo XC90 T8 Hybrid         |
| Clóvis Carrasco           | Viviana Sequeira              | 2019-06-22                    | 1860.0                  | Volvo XC90 T8 Hybrid         |
| Hipólito Granja           | Emídio Dornelles              | 2019-05-29                    | 2054.0                  | Volvo XC90 T8 Hybrid         |
| Noêmia   Tupinambá        | Roque Vásquez                 | 2019-06-12                    | 1939.0                  | Volvo XC90 T8 Hybrid         |
+---------------------------+-------------------------------+-------------------------------+-------------------------+------------------------------+--+
100 rows selected (0.869 seconds)
```

<br>
<br>

Agora vou criar a tabela "locacaoanalitico2" sem o particionamento, mas com o **bucketing**, adicionando os comandos **"CLUSTERED BY (veiculo) INTO 4 BUCKETS;"**, e popular a mesma com os mesmos dados que a tabela locacaoanalitico. 

```
0: jdbc:hive2://> CREATE TABLE locacaoanalitico2 (cliente string, despachante string, datalocacao date, total double, veiculo string)
. . . . . . . . > CLUSTERED BY (veiculo) INTO 4 BUCKETS;
OK
No rows affected (1.091 seconds)
0: jdbc:hive2://> INSERT OVERWRITE TABLE locacaoanalitico2
. . . . . . . . > SELECT cli.nome, des.nome, loc.datalocacao, loc.total, veic.modelo
. . . . . . . . > FROM locacao loc
. . . . . . . . > JOIN despachantes des ON (loc.iddespachante = des.iddespachante)
. . . . . . . . > JOIN clientes cli ON (loc.idcliente = cli.idcliente)
. . . . . . . . > JOIN veiculos veic ON (loc.idveiculo = veic.idveiculo);
23/01/05 08:59:28 [main]: ERROR calcite.RelOptHiveTable: No Stats for locacao@despachantes, Columns: iddespachante
23/01/05 08:59:28 [main]: ERROR parse.CalcitePlanner: CBO failed due to missing column stats (see previous errors), skipping CBO
Query ID = cloudera_20230105085959_88803078-726a-44a1-b3be-6a3608b8f77d
Total jobs = 1
Execution log at: /tmp/cloudera/cloudera_20230105085959_88803078-726a-44a1-b3be-6a3608b8f77d.log
Execution log at: /tmp/cloudera/cloudera_20230105085959_88803078-726a-44a1-b3be-6a3608b8f77d.log
2023-01-05 08:59:39	Starting to launch local task to process map join;	maximum memory = 932184064
2023-01-05 08:59:44	Dump the side-table for tag: 1 with group count: 30 into file: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_08-59-28_086_2761020435470148145-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile31--.hashtable
2023-01-05 08:59:44	Uploaded 1 File to: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_08-59-28_086_2761020435470148145-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile31--.hashtable (1458 bytes)
2023-01-05 08:59:44	Dump the side-table for tag: 1 with group count: 25 into file: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_08-59-28_086_2761020435470148145-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile41--.hashtable
2023-01-05 08:59:44	Uploaded 1 File to: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_08-59-28_086_2761020435470148145-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile41--.hashtable (1132 bytes)
2023-01-05 08:59:44	Dump the side-table for tag: 1 with group count: 10 into file: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_08-59-28_086_2761020435470148145-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile51--.hashtable
2023-01-05 08:59:44	Uploaded 1 File to: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_08-59-28_086_2761020435470148145-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile51--.hashtable (622 bytes)
2023-01-05 08:59:44	End of local task; Time Taken: 4.459 sec.
2023-01-05 08:59:39	Starting to launch local task to process map join;	maximum memory = 932184064
2023-01-05 08:59:44	Dump the side-table for tag: 1 with group count: 30 into file: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_08-59-28_086_2761020435470148145-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile31--.hashtable
2023-01-05 08:59:44	Uploaded 1 File to: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_08-59-28_086_2761020435470148145-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile31--.hashtable (1458 bytes)
2023-01-05 08:59:44	Dump the side-table for tag: 1 with group count: 25 into file: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_08-59-28_086_2761020435470148145-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile41--.hashtable
2023-01-05 08:59:44	Uploaded 1 File to: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_08-59-28_086_2761020435470148145-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile41--.hashtable (1132 bytes)
2023-01-05 08:59:44	Dump the side-table for tag: 1 with group count: 10 into file: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_08-59-28_086_2761020435470148145-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile51--.hashtable
2023-01-05 08:59:44	Uploaded 1 File to: file:/tmp/cloudera/9a7a8961-7997-433f-8e0a-ddcd0c0f721a/hive_2023-01-05_08-59-28_086_2761020435470148145-1/-local-10004/HashTable-Stage-8/MapJoin-mapfile51--.hashtable (622 bytes)
2023-01-05 08:59:44	End of local task; Time Taken: 4.459 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks is set to 0 since there's no reduce operator
23/01/05 08:59:46 [HiveServer2-Background-Pool: Thread-91]: WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
Starting Job = job_1671629034454_0022, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1671629034454_0022/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1671629034454_0022
Hadoop job information for Stage-8: number of mappers: 1; number of reducers: 0
23/01/05 09:00:02 [HiveServer2-Background-Pool: Thread-91]: WARN mapreduce.Counters: Group org.apache.hadoop.mapred.Task$Counter is deprecated. Use org.apache.hadoop.mapreduce.TaskCounter instead
2023-01-05 09:00:02,085 Stage-8 map = 0%,  reduce = 0%
2023-01-05 09:00:22,195 Stage-8 map = 100%,  reduce = 0%, Cumulative CPU 4.43 sec
MapReduce Total cumulative CPU time: 4 seconds 430 msec
Ended Job = job_1671629034454_0022
Loading data to table locacao.locacaoanalitico2
Table locacao.locacaoanalitico2 stats: [numFiles=1, numRows=100, totalSize=7146, rawDataSize=7046]
MapReduce Jobs Launched: 
Stage-Stage-8: Map: 1   Cumulative CPU: 4.43 sec   HDFS Read: 14246 HDFS Write: 7230 SUCCESS
Total MapReduce CPU Time Spent: 4 seconds 430 msec
OK
No rows affected (59.789 seconds)
```

<br>
<br>

E ao fazermos o **"SELECT * FROM TABELAOANALITICO2"** podemos ver que a execução do comando leva agora **0.357 segundos**.

```
0: jdbc:hive2://> SELECT * FROM locacaoanalitico2;
OK
+----------------------------+--------------------------------+--------------------------------+--------------------------+------------------------------+--+
| locacaoanalitico2.cliente  | locacaoanalitico2.despachante  | locacaoanalitico2.datalocacao  | locacaoanalitico2.total  |  locacaoanalitico2.veiculo   |
+----------------------------+--------------------------------+--------------------------------+--------------------------+------------------------------+--+
| Carminda Veríssimo         | Graça Ornellas                 | 2019-02-20                     | 1996.0                   | Infiniti Q50S 3.7 Sedan      |
| Clóvis Carrasco            | Graça Ornellas                 | 2019-06-30                     | 1843.0                   | BMW 750i xDrive              |
| Rebeca Torcuato            | Graça Ornellas                 | 2019-03-09                     | 2016.0                   | BMW 750i xDrive              |
| Xisto Mendoça              | Emídio Dornelles               | 2019-04-19                     | 1857.0                   | Volvo XC90 T8 Hybrid         |
| Hipólito Granja            | Viviana Sequeira               | 2019-02-24                     | 2049.0                   | BMW 750i xDrive              |
| Márcio Taveira             | Graça Ornellas                 | 2019-05-15                     | 1986.0                   | BMW 750i xDrive              |
| Micael Mangueira           | Deolinda Vilela                | 2019-05-01                     | 1989.0                   | Volvo XC90 T8 Hybrid         |
| Vanderlei Açores           | Graça Ornellas                 | 2019-03-29                     | 1874.0                   | Infiniti Q50S 3.7 Sedan      |
| Rebeca Torcuato            | Noêmia   Orriça                | 2019-05-10                     | 2027.0                   | Mercedes-Benz S65 AMG Coupe  |
| Alvito Espargosa           | Deolinda Vilela                | 2019-03-17                     | 2010.0                   | Tesla Model S P90D           |
| Iuri Alancastre            | Graça Ornellas                 | 2019-05-12                     | 2006.0                   | Mercedes-Benz S65 AMG Coupe  |
| Sabino Abrantes            | Viviana Sequeira               | 2019-02-26                     | 1915.0                   | Volvo XC90 T8 Hybrid         |
| Severino Leiria            | Matilde Rebouças               | 2019-06-12                     | 2089.0                   | BMW 750i xDrive              |
| Xisto Mendoça              | Graça Ornellas                 | 2019-06-04                     | 1942.0                   | BMW 750i xDrive              |
| Isaura Farias              | Viviana Sequeira               | 2019-02-22                     | 2071.0                   | Tesla Model S P90D           |
| Rebeca Torcuato            | Felisbela Dornelles            | 2019-05-29                     | 2035.0                   | Mercedes-Benz S65 AMG Coupe  |
| Cleusa Lamego              | Carminda Pestana               | 2019-04-09                     | 1810.0                   | Infiniti Q50S 3.7 Sedan      |
| Iara Cardoso               | Roque Vásquez                  | 2019-04-21                     | 2017.0                   | Infiniti Q50S 3.7 Sedan      |
| Carminda Veríssimo         | Noêmia   Orriça                | 2019-05-11                     | 2095.0                   | Infiniti Q50S 3.7 Sedan      |
| Alvito Espargosa           | Viviana Sequeira               | 2019-06-17                     | 2020.0                   | Volvo XC90 T8 Hybrid         |
| Micael Mangueira           | Matilde Rebouças               | 2019-04-12                     | 2077.0                   | Tesla Model S P90D           |
| Paula Padilha              | Matilde Rebouças               | 2019-06-02                     | 1867.0                   | Infiniti Q50S 3.7 Sedan      |
| Sabino Abrantes            | Uriel Queiroz                  | 2019-05-12                     | 1862.0                   | Volvo XC90 T8 Hybrid         |
| Micael Mangueira           | Matilde Rebouças               | 2019-04-06                     | 1855.0                   | Volvo XC90 T8 Hybrid         |
| Tobias Garcés              | Felisbela Dornelles            | 2019-04-26                     | 2033.0                   | Tesla Model S P90D           |
| Zeferino Matoso            | Viviana Sequeira               | 2019-03-16                     | 2034.0                   | Infiniti Q50S 3.7 Sedan      |
| Hipólito Granja            | Deolinda Vilela                | 2019-05-06                     | 1865.0                   | BMW 750i xDrive              |
| Noêmia   Tupinambá         | Emídio Dornelles               | 2019-06-05                     | 1997.0                   | Tesla Model S P90D           |
| Vanderlei Açores           | Noêmia   Orriça                | 2019-05-07                     | 2032.0                   | Infiniti Q50S 3.7 Sedan      |
| Micael Mangueira           | Emídio Dornelles               | 2019-05-06                     | 2011.0                   | BMW 750i xDrive              |
| Márcio Taveira             | Carminda Pestana               | 2019-03-26                     | 1867.0                   | Tesla Model S P90D           |
| Sabino Abrantes            | Uriel Queiroz                  | 2019-03-14                     | 1963.0                   | Mercedes-Benz S65 AMG Coupe  |
| Cátia Fróis                | Carminda Pestana               | 2019-04-15                     | 1845.0                   | Volvo XC90 T8 Hybrid         |
| Micael Mangueira           | Deolinda Vilela                | 2019-06-28                     | 2020.0                   | BMW 750i xDrive              |
| Noêmia   Tupinambá         | Felisbela Dornelles            | 2019-06-22                     | 1938.0                   | Volvo XC90 T8 Hybrid         |
| Clóvis Carrasco            | Carminda Pestana               | 2019-06-01                     | 1980.0                   | BMW 750i xDrive              |
| Iuri Alancastre            | Felisbela Dornelles            | 2019-03-21                     | 2037.0                   | Volvo XC90 T8 Hybrid         |
| Xisto Mendoça              | Matilde Rebouças               | 2019-05-31                     | 1970.0                   | BMW 750i xDrive              |
| Emílio Faro                | Viviana Sequeira               | 2019-05-02                     | 1900.0                   | BMW 750i xDrive              |
| Tobias Garcés              | Carminda Pestana               | 2019-06-15                     | 1888.0                   | BMW 750i xDrive              |
| Rebeca Torcuato            | Viviana Sequeira               | 2019-02-20                     | 2042.0                   | Mercedes-Benz S65 AMG Coupe  |
| Iara Cardoso               | Carminda Pestana               | 2019-03-04                     | 1821.0                   | Infiniti Q50S 3.7 Sedan      |
| Cleusa Lamego              | Emídio Dornelles               | 2019-05-21                     | 2009.0                   | Tesla Model S P90D           |
| Severino Leiria            | Carminda Pestana               | 2019-04-02                     | 1837.0                   | Infiniti Q50S 3.7 Sedan      |
| Micael Mangueira           | Uriel Queiroz                  | 2019-06-08                     | 1971.0                   | Tesla Model S P90D           |
| Cátia Fróis                | Emídio Dornelles               | 2019-05-28                     | 1818.0                   | Tesla Model S P90D           |
| Noêmia   Tupinambá         | Graça Ornellas                 | 2019-02-22                     | 2031.0                   | Volvo XC90 T8 Hybrid         |
| Iara Cardoso               | Noêmia   Orriça                | 2019-06-04                     | 1938.0                   | Infiniti Q50S 3.7 Sedan      |
| Alvito Espargosa           | Deolinda Vilela                | 2019-03-08                     | 1975.0                   | Infiniti Q50S 3.7 Sedan      |
| Vicente Rosa               | Viviana Sequeira               | 2019-02-27                     | 2014.0                   | Tesla Model S P90D           |
| Rebeca Torcuato            | Viviana Sequeira               | 2019-03-15                     | 1958.0                   | Volvo XC90 T8 Hybrid         |
| Tobias Garcés              | Viviana Sequeira               | 2019-05-11                     | 2024.0                   | Tesla Model S P90D           |
| Laura Marcondes            | Carminda Pestana               | 2019-04-20                     | 1946.0                   | Volvo XC90 T8 Hybrid         |
| Vicente Rosa               | Matilde Rebouças               | 2019-05-07                     | 1855.0                   | Infiniti Q50S 3.7 Sedan      |
| Micael Mangueira           | Graça Ornellas                 | 2019-03-05                     | 2039.0                   | Volvo XC90 T8 Hybrid         |
| Paula Padilha              | Emídio Dornelles               | 2019-03-18                     | 1991.0                   | BMW 750i xDrive              |
| Carminda Veríssimo         | Graça Ornellas                 | 2019-03-18                     | 1914.0                   | Infiniti Q50S 3.7 Sedan      |
| Emílio Faro                | Graça Ornellas                 | 2019-04-21                     | 1890.0                   | BMW 750i xDrive              |
| Vicente Rosa               | Noêmia   Orriça                | 2019-06-21                     | 1840.0                   | Volvo XC90 T8 Hybrid         |
| Noêmia   Tupinambá         | Carminda Pestana               | 2019-03-27                     | 2004.0                   | Mercedes-Benz S65 AMG Coupe  |
| Iara Cardoso               | Deolinda Vilela                | 2019-03-08                     | 2075.0                   | Volvo XC90 T8 Hybrid         |
| Vicente Rosa               | Graça Ornellas                 | 2019-05-10                     | 1838.0                   | Volvo XC90 T8 Hybrid         |
| Clóvis Carrasco            | Uriel Queiroz                  | 2019-05-21                     | 1974.0                   | BMW 750i xDrive              |
| Vicente Rosa               | Felisbela Dornelles            | 2019-02-26                     | 2064.0                   | Infiniti Q50S 3.7 Sedan      |
| Vanderlei Açores           | Roque Vásquez                  | 2019-05-22                     | 1836.0                   | BMW 750i xDrive              |
| Iuri Alancastre            | Felisbela Dornelles            | 2019-04-20                     | 1800.0                   | Volvo XC90 T8 Hybrid         |
| Blasco Canto               | Noêmia   Orriça                | 2019-06-22                     | 2070.0                   | BMW 750i xDrive              |
| Cleusa Lamego              | Graça Ornellas                 | 2019-04-11                     | 2001.0                   | BMW 750i xDrive              |
| Iuri Alancastre            | Matilde Rebouças               | 2019-02-20                     | 1858.0                   | Volvo XC90 T8 Hybrid         |
| Paula Padilha              | Felisbela Dornelles            | 2019-05-26                     | 1983.0                   | Infiniti Q50S 3.7 Sedan      |
| Cátia Fróis                | Carminda Pestana               | 2019-06-08                     | 1853.0                   | BMW 750i xDrive              |
| Emílio Faro                | Deolinda Vilela                | 2019-06-14                     | 1813.0                   | Mercedes-Benz S65 AMG Coupe  |
| Iuri Alancastre            | Noêmia   Orriça                | 2019-03-27                     | 1950.0                   | Mercedes-Benz S65 AMG Coupe  |
| Cátia Fróis                | Graça Ornellas                 | 2019-06-23                     | 1831.0                   | Volvo XC90 T8 Hybrid         |
| Severino Leiria            | Uriel Queiroz                  | 2019-06-03                     | 2049.0                   | Mercedes-Benz S65 AMG Coupe  |
| Emílio Faro                | Roque Vásquez                  | 2019-05-13                     | 1843.0                   | BMW 750i xDrive              |
| Xisto Mendoça              | Carminda Pestana               | 2019-06-26                     | 1881.0                   | BMW 750i xDrive              |
| Rosana Betancour           | Graça Ornellas                 | 2019-05-19                     | 1810.0                   | Mercedes-Benz S65 AMG Coupe  |
| Emílio Faro                | Noêmia   Orriça                | 2019-04-28                     | 1899.0                   | Infiniti Q50S 3.7 Sedan      |
| Paula Padilha              | Carminda Pestana               | 2019-06-15                     | 1812.0                   | BMW 750i xDrive              |
| Vanderlei Açores           | Deolinda Vilela                | 2019-04-08                     | 1921.0                   | Infiniti Q50S 3.7 Sedan      |
| Iara Cardoso               | Matilde Rebouças               | 2019-04-03                     | 1945.0                   | Volvo XC90 T8 Hybrid         |
| Iuri Alancastre            | Matilde Rebouças               | 2019-04-01                     | 1990.0                   | Volvo XC90 T8 Hybrid         |
| Cátia Fróis                | Roque Vásquez                  | 2019-04-16                     | 1956.0                   | Mercedes-Benz S65 AMG Coupe  |
| Vicente Rosa               | Viviana Sequeira               | 2019-04-05                     | 1927.0                   | Volvo XC90 T8 Hybrid         |
| Vicente Rosa               | Matilde Rebouças               | 2019-04-23                     | 1801.0                   | Mercedes-Benz S65 AMG Coupe  |
| Iuri Alancastre            | Matilde Rebouças               | 2019-03-22                     | 1802.0                   | BMW 750i xDrive              |
| Iara Cardoso               | Graça Ornellas                 | 2019-02-27                     | 1851.0                   | Volvo XC90 T8 Hybrid         |
| Paula Padilha              | Felisbela Dornelles            | 2019-06-22                     | 2089.0                   | Volvo XC90 T8 Hybrid         |
| Emílio Faro                | Graça Ornellas                 | 2019-03-27                     | 1967.0                   | Mercedes-Benz S65 AMG Coupe  |
| Tobias Garcés              | Carminda Pestana               | 2019-05-07                     | 1826.0                   | BMW 750i xDrive              |
| Xisto Mendoça              | Uriel Queiroz                  | 2019-05-13                     | 1864.0                   | BMW 750i xDrive              |
| Micael Mangueira           | Graça Ornellas                 | 2019-03-10                     | 2056.0                   | BMW 750i xDrive              |
| Rosana Betancour           | Matilde Rebouças               | 2019-05-22                     | 1986.0                   | Mercedes-Benz S65 AMG Coupe  |
| Vanderlei Açores           | Deolinda Vilela                | 2019-02-21                     | 1861.0                   | BMW 750i xDrive              |
| Clóvis Carrasco            | Viviana Sequeira               | 2019-06-22                     | 1860.0                   | Volvo XC90 T8 Hybrid         |
| Paula Padilha              | Uriel Queiroz                  | 2019-02-27                     | 2100.0                   | Tesla Model S P90D           |
| Hipólito Granja            | Emídio Dornelles               | 2019-05-29                     | 2054.0                   | Volvo XC90 T8 Hybrid         |
| Alvito Espargosa           | Emídio Dornelles               | 2019-03-30                     | 1886.0                   | BMW 750i xDrive              |
| Noêmia   Tupinambá         | Roque Vásquez                  | 2019-06-12                     | 1939.0                   | Volvo XC90 T8 Hybrid         |
+----------------------------+--------------------------------+--------------------------------+--------------------------+------------------------------+--+
100 rows selected (0.357 seconds)
```


<br>
<br>

Finalizo a descrição desse script aqui. Acrescento que são minhas observações durante meu aprendizado e que ainda estou no início, então peço paciência sobre qualquer erro (= Obrigada pela compreensão :smile:

Obrigada por ler até o final! :wink:




