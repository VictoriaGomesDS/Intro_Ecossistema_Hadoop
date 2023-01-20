# Intro_Ecossistema_Hadoop
<br>

Um pouco sobre meus novos conhecimentos em "SQL para Big Data: Ecossistema Hadoop com Hive e Impala".


![Hadoop_Ecossystem](https://user-images.githubusercontent.com/102270053/211628037-eee16df8-7d5e-458f-946c-800453c97685.jpg)

## Introdução :elephant:

Por mais de duas semanas me dediquei a estudar sobre o ecossistema hadoop e diversas ferramentas. Durante o curso "SQL para Big Data: Ecossistema Hadoop com Hive e Impala", oferecido pelo instrutor Fernando Amaral na plataforma de ensino "EIA - Escola de Inteligência Artificial".

<br>

Neste curso pude desenvolver e aperfeiçoar conhecimentos acerca do Ecossistema Hadoop:
- Manuseando querys com HiveQL no prompt do Hive;
- Manuseando querys com HiveQL no prompt do Impala;
- Estudar a infraestrutura do Hive e Impala;
- Ingestão de dados estruturados com SQOOP e carregando dados no/do HDFS;
- Tipos de dados;
- ETC.

<br>

Pude praticar formas de otimização do hive e do HDFS através de métodos como:
- Particionamento;
- Bucketing;
- Views e tabelas temporárias;
- ORC e Parquet;
- Vetorização;
- CBO;
- Hive com Spark.

<br>

E por último pude verificar mais sobre acesso e querys com hive e impala do HUE.

![print_hue](https://user-images.githubusercontent.com/102270053/211631861-75598ed3-afca-452b-bc80-b98ee8492622.png)

<br>

## Materiais :bookmark_tabs:

- **README:** Entendendo o projeto;

| Título                      | Conteúdo                                                                                                                                                                                   |
|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1. **Primeiros_Passos:**    | Copiando arquivos da pasta compartilhada para o HDFS, acessando e criando um  database no Hive e copiando os arquivos do HDFS para tabelas no Hive.                                        |
| 2. **HiveQL:**              | Demonstrando algumas querys manipulando os dados das tabelas do Hive.                                                                                                                      |
| 3. **SQOOP:**               | Script mostrando eu me conectando as tabelas do banco mysql, e copiando as  tabelas, já existentes lá, em um novo banco no Hive.                                                           |
| 4. **Partition_Bucketing:** | Script mostrando um exemplo de particionamento.                                                                                                                                            |
| 5. **ORC:**                 | Script mostrando um exemplo de criação de arquivo ORC.                                                                                                                                     |
| 6. **Vetorização:**         | Script mostrando a diferença da execução de uma query com a vetorização  desativada e ativada.                                                                                             |
| 7. **COB_Cost_Based:**      | Script mostrando a diferença do tempo de execução quando utilizamos essa técnica de otimização de consultas baseada na coleta de dados estatísticos para  aperfeiçoar o plano de execução. |
| 8. **Hive_com_Spark:**      | Script mostrando a diferença do tempo de execução quando mudamos a engine de  processamento de dados do padrão Map Reduce para o Spark para a otimização de  consultas.                    |
| 9. **Impala:**              | Mostrando um pouco do script de conexão e querys no Impala pelo prompt do linux.                                                                                                           |


<br>

## Softwares e Programas :computer:

- Oracle VM VirtualBox;
- Cloudera Quickstart VM (CDH);

Através de uma Máquina Virtual, da Oracle VM VirtualBox, criada pela importação da appliance Cloudera Quickstart VM, obtive acesso a distribuição 100% de código aberto da Cloudera, incluindo o Apache Hadoop e o Cloudera Manager. Eles são ambientes ideais para aprender sobre o Hadoop, com diversas ferramentas já pré-instaladas para demonstrarmos nossos conhecimentos.

Lá tive acesso a uma máquina Red Hat Linux, distribuição de Linux, onde através do cmd do linux consegui executar a maior parte das querys no hive e impala. 

Também criei uma pasta compartilhada entre minha máquina e a VM.

![print_inicial_cloud](https://user-images.githubusercontent.com/102270053/211658340-23e5e3ac-be3e-4761-8cba-755215e08297.png)

<br>

## Linguagens e Metodologia :pencil:

- HiveQL;
- SQL;
- Linguagem do prompt de comando do Linux.

**Exemplo:** :mag:
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

## Considerações finais :mortar_board:

Gostei muito de estudar sobre o ecossistema e, com certeza, ainda irei investir muito sobre o mesmo e em ferramentas paralelas de outras empresas. 
Foi um curso com uma didádica muito boa, mesmo que ainda bem introdutória, mas que com certeza foi uma ótima forma de me introduzir a temática.

Ainda há diversas coisas que quero mudar, como: ao invés de fazer em VM fazer em um container gerado através de uma imagem da cloudera. Com certeza está nos meus planos :wink:

<br>
 
## Fontes :point_left:

- https://www.eia.ai/sql-para-big-data-ecossistema-hadoop-com-hive-e-impala
- https://rodrigolampier.medium.com/machine-learning-em-larga-escala-funcionamento-do-spark-730fa44eac5d
- https://towardsdatascience.com/understanding-apache-parquet-7197ba6462a9
- https://www.upsolver.com/blog/the-file-format-fundamentals-of-big-data#:~:text=Optimized%20Row%20Columnar%20(ORC)%20is,Columnar%20File%20(RCFile)%20format.
- https://docs.informatica.com/pt_pt/data-quality-and-governance/data-quality/10-5-1/novidades-e-mudancas--10-5-1-/parte---7--versao-10-2---10-2-hotfix-2/novidades-da-versao-10-2/big-data/funcionalidade-do-hive-para-o-mecanismo-spark.html
- https://medium.com/@CloudEtl/como-usar-udfs-do-hive-no-spark-sql-614717ac124e
- https://www.okera.com/blogs/bucketing-in-hive-example-with-okera/
- https://docs.cloudera.com/HDPDocuments/HDP2/HDP-2.6.3/bk_data-access/content/query-vectorization.html

***São essas que tenho registrado, além de minhas próprias anotações do curso (=***




