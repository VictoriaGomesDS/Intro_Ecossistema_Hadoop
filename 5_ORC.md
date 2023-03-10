# Explicando o desenvolvimento do Script "ORC" :pencil:

Esse foi o script onde mostro um pouco do desenvolvimento de uma tabela salva como um arquivo ORC.

<br>

![image](https://user-images.githubusercontent.com/102270053/213730420-53f770c9-8929-41e5-b8c6-02da5fa27b33.png)

<br>

## Overview ORC

ORC, sigla de Optimized Row Columnar, nada mais é que um formato de arquivo binário colunar que melhora o desempenho de como o Hive lê, grava e processa os dados, otimizado principalmente para analise. Tendo, também, como diferêncial, sua incrível capacidade de "compressão e descompressão".

Este formato de arquivo armazena coleções de dados de modelo linear em um único arquivo em formato colunar, assim permitindo o processamento paralelo de coleções de linhas em um cluster, tendo em vista que os arquivos passam a ser organizados em faixas independentes de dados.

<br>

> Futuramente posso falar, também, sobre arquivos Parquet.

<br>
<br>

## Analise do código

<br>

> Podemos identificar como os comandos que inseri aquelas instruções que estão a frente do indicador "0: jdbc:hive2://> ".

<br>

Aqui, já logada no Hive e no database locacao, crio a tabela **"clientes_orc"** e indico que ela deve ser "armazenada" como um arquivo ORC **"STORED AS orc;"**
E, logo após a criação da tabela, já populo a mesma com os dados da tabela clientes.

```
0: jdbc:hive2://> CREATE EXTERNAL TABLE clientes_orc (idcliente int, cnh string, cpf string, validadecnh date, nome string, datacadastro date, datanascimento date, telefone string, status string)
. . . . . . . . > STORED AS orc;
OK
No rows affected (0.74 seconds)
0: jdbc:hive2://> INSERT OVERWRITE TABLE clientes_orc SELECT * FROM clientes;
Query ID = cloudera_20230105104444_f22ab453-2a9f-43e3-ac3f-dabc8f8c5e8a
Total jobs = 1
Launching Job 1 out of 1
Number of reduce tasks is set to 0 since there's no reduce operator
23/01/05 10:44:46 [HiveServer2-Background-Pool: Thread-34]: WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
Starting Job = job_1671629034454_0023, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1671629034454_0023/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1671629034454_0023
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 0
23/01/05 10:45:21 [HiveServer2-Background-Pool: Thread-34]: WARN mapreduce.Counters: Group org.apache.hadoop.mapred.Task$Counter is deprecated. Use org.apache.hadoop.mapreduce.TaskCounter instead
2023-01-05 10:45:21,654 Stage-1 map = 0%,  reduce = 0%
2023-01-05 10:45:44,288 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 2.62 sec
MapReduce Total cumulative CPU time: 2 seconds 620 msec
Ended Job = job_1671629034454_0023
Stage-4 is selected by condition resolver.
Stage-3 is filtered out by condition resolver.
Stage-5 is filtered out by condition resolver.
Moving data to: hdfs://quickstart.cloudera:8020/user/hive/warehouse/locacao.db/clientes_orc/.hive-staging_hive_2023-01-05_10-44-43_267_6373117385226245115-1/-ext-10000
Loading data to table locacao.clientes_orc
Table locacao.clientes_orc stats: [numFiles=1, numRows=25, totalSize=1967, rawDataSize=16100]
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1   Cumulative CPU: 2.62 sec   HDFS Read: 6966 HDFS Write: 2046 SUCCESS
Total MapReduce CPU Time Spent: 2 seconds 620 msec
OK
No rows affected (65.871 seconds)
```

<br>
<br>

Então faço uma query selecionando todos os dados da tabela recém criada.

```
0: jdbc:hive2://> SELECT * FROM clientes_orc;
OK
+-------------------------+-------------------+-------------------+---------------------------+---------------------+----------------------------+------------------------------+------------------------+----------------------+--+
| clientes_orc.idcliente  | clientes_orc.cnh  | clientes_orc.cpf  | clientes_orc.validadecnh  |  clientes_orc.nome  | clientes_orc.datacadastro  | clientes_orc.datanascimento  | clientes_orc.telefone  | clientes_orc.status  |
+-------------------------+-------------------+-------------------+---------------------------+---------------------+----------------------------+------------------------------+------------------------+----------------------+--+
| 1                       | 16528447080       | 44971318961       | 2021-09-30                | Alvito Espargosa    | 2019-01-25                 | 1973-03-15                   | 51390834589            | Ativo                |
| 2                       | 83327244065       | 44976832700       | 2028-02-24                | Blasco Canto        | 2019-01-15                 | 1969-10-31                   | 51922375520            | Ativo                |
| 3                       | 54513828080       | 27672609233       | 2024-12-11                | Carminda Veríssimo  | 2019-04-09                 | 1967-11-03                   | 51738662107            | Ativo                |
| 4                       | 54075418316       | 75523770439       | 2025-08-02                | Cleusa Lamego       | 2019-05-26                 | 1961-10-26                   | 51824283267            | Ativo                |
| 5                       | 41148381120       | 55079718160       | 2020-08-21                | Clóvis Carrasco     | 2019-01-28                 | 1974-11-02                   | 51056509510            | Ativo                |
| 6                       | 85588747194       | 30471250272       | 2020-06-05                | Cátia Fróis         | 2019-05-23                 | 1965-07-08                   | 51901121962            | Ativo                |
| 7                       | 10003401804       | 45948223160       | 2027-05-27                | Emílio Faro         | 2019-04-17                 | 1966-09-23                   | 51608924744            | Ativo                |
| 8                       | 41555470319       | 15942100562       | 2026-05-16                | Hipólito Granja     | 2019-05-13                 | 1974-09-21                   | 51020081027            | Ativo                |
| 9                       | 86405032817       | 43316295637       | 2027-11-25                | Iara Cardoso        | 2019-04-04                 | 1981-06-18                   | 51815002282            | Ativo                |
| 10                      | 15516586747       | 76742255766       | 2021-12-25                | Isaura Farias       | 2019-05-04                 | 1971-04-22                   | 51081565935            | Ativo                |
| 11                      | 44818765309       | 63408388553       | 2027-05-03                | Iuri Alancastre     | 2019-02-18                 | 1970-07-15                   | 51505707400            | Ativo                |
| 12                      | 11257675109       | 41824436778       | 2020-05-22                | Laura Marcondes     | 2019-06-30                 | 1985-08-29                   | 51532760959            | Ativo                |
| 13                      | 88357618405       | 49548848905       | 2021-03-25                | Micael Mangueira    | 2019-05-24                 | 1994-01-08                   | 51050644140            | Ativo                |
| 14                      | 58831402110       | 84776516129       | 2026-03-08                | Márcio Taveira      | 2019-02-13                 | 1963-06-07                   | 51545396437            | Ativo                |
| 15                      | 86405301141       | 89650561775       | 2027-05-06                | Noêmia   Tupinambá  | 2019-06-28                 | 1987-03-12                   | 51073079084            | Ativo                |
| 16                      | 70686300220       | 95360639928       | 2028-07-19                | Paula Padilha       | 2019-04-09                 | 1996-05-16                   | 51468982948            | Ativo                |
| 17                      | 70805566600       | 61460867546       | 2022-05-31                | Rebeca Torcuato     | 2019-02-09                 | 1994-12-06                   | 51398031738            | Ativo                |
| 18                      | 84611132765       | 85761272593       | 2023-03-24                | Rosana Betancour    | 2019-05-10                 | 1976-06-08                   | 51298196500            | Ativo                |
| 19                      | 72887644190       | 40239379806       | 2022-09-06                | Sabino Abrantes     | 2019-05-27                 | 1982-02-16                   | 51805190959            | Ativo                |
| 20                      | 73201278572       | 56448355307       | 2025-02-02                | Severino Leiria     | 2019-02-15                 | 1965-08-06                   | 51837010991            | Ativo                |
| 21                      | 35215454663       | 33281183954       | 2027-07-26                | Tobias Garcés       | 2019-06-15                 | 1962-08-20                   | 51038698605            | Ativo                |
| 22                      | 77853823525       | 55351556215       | 2028-08-14                | Vanderlei Açores    | 2019-01-18                 | 1998-05-06                   | 51069055089            | Inativo              |
| 23                      | 05013312205       | 32675365584       | 2021-12-15                | Vicente Rosa        | 2019-05-14                 | 1983-11-09                   | 51782871990            | Ativo                |
| 24                      | 57460105538       | 54721641167       | 2028-02-03                | Xisto Mendoça       | 2019-06-06                 | 1969-07-27                   | 51862632282            | Ativo                |
| 25                      | 02251571280       | 83497776322       | 2025-02-08                | Zeferino Matoso     | 2019-05-01                 | 1973-03-23                   | 51230738729            | Ativo                |
+-------------------------+-------------------+-------------------+---------------------------+---------------------+----------------------------+------------------------------+------------------------+----------------------+--+
25 rows selected (0.507 seconds)
```

<br>
<br>

Finalizo a descrição desse script aqui. Acrescento que são minhas observações durante meu aprendizado e que ainda estou no início, então peço paciência sobre qualquer erro (= Obrigada pela compreensão :smile:

Obrigada por ler até o final! :wink:




