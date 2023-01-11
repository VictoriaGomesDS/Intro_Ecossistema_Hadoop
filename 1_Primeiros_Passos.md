# Explicando o desenvolvimento do Script "Primeiros_Passos" :pencil:

Esse foi o script onde inicializei as propostas do curso. Nele copio as bases de dados da minha pasta compartilhada local para o hdfs, para que posteriormente consiga copiar os mesmos do HDFS para as tabelas que criei no Hive.

<br>

> Podemos identificar como os comandos que inseri aquelas instruções que estão a frente do indicador "[cloudera@quickstart ~]$".

<br>

Abaixo listei os recursos de mídia da VM para ter certeza de que já havia configurado meu acesso a ela.

```
[cloudera@quickstart ~]$ cd /media
[cloudera@quickstart media]$ ls
cdrom  sf_PastaCompartilhada  VBox_GAs_7.0.4
[cloudera@quickstart media]$ ls -al
total 15
drwxr-xr-x.  5 root     root     4096 Dec 15 05:03 .
drwxrwxr-x. 22 root     root     4096 Dec 15 02:00 ..
drwxr-xr-x   2 root     root     4096 Jul 19  2017 cdrom
drwxrwx---   1 root     vboxsf      0 Dec 15 04:25 sf_PastaCompartilhada
dr-xr-xr-x   5 cloudera cloudera 2570 Nov 16 09:05 VBox_GAs_7.0.4
[cloudera@quickstart media]$ cd sf_PastaCompartilhada/
```

<br>

Então confirmado meu acesso à pasta, em seguida listei os arquivos e pastas presentes no HDFS, entrei no diretório de user cloudera e criei o diretório "locacao" para que no futuro possa copiar os arquivos das bases de dados para dentro do mesmo. Então entro no diretório da pasta compartilhada e confiro os arquivos.

```
[cloudera@quickstart sf_PastaCompartilhada]$ hdfs dfs -ls /
Found 6 items
drwxrwxrwx   - hdfs  supergroup          0 2017-07-19 05:34 /benchmarks
drwxr-xr-x   - hbase supergroup          0 2022-12-15 05:01 /hbase
drwxr-xr-x   - solr  solr                0 2017-07-19 05:37 /solr
drwxrwxrwt   - hdfs  supergroup          0 2022-12-14 09:18 /tmp
drwxr-xr-x   - hdfs  supergroup          0 2017-07-19 05:36 /user
drwxr-xr-x   - hdfs  supergroup          0 2017-07-19 05:36 /var
[cloudera@quickstart sf_PastaCompartilhada]$ cd /user
[cloudera@quickstart sf_PastaCompartilhada]$ hdfs dfs -mkdir /user/cloudera/locacao
[cloudera@quickstart sf_PastaCompartilhada]$ ..
bash: ..: command not found
[cloudera@quickstart sf_PastaCompartilhada]$ cd ..
[cloudera@quickstart media]$ cd ..
[cloudera@quickstart /]$ cd /home/cloudera/Downloads/arquivos/
[cloudera@quickstart arquivos]$ ls -al
total 68
drwxrwx--- 2 cloudera cloudera  4096 Dec 15 05:07 .
drwxr-xr-x 3 cloudera cloudera  4096 Dec 15 05:10 ..
-rwxrwx--- 1 cloudera cloudera  2365 Nov 22 05:16 clientes.csv
-rwxrwx--- 1 cloudera cloudera   390 Nov 22 05:16 despachantes.csv
-rwxrwx--- 1 cloudera cloudera   396 Nov 22 05:16 incluir hive-site
-rwxrwx--- 1 cloudera cloudera  3833 Nov 22 05:16 locacao.csv
-rwxrwx--- 1 cloudera cloudera 40329 Nov 22 05:16 relacional.png
-rwxrwx--- 1 cloudera cloudera  1939 Nov 22 05:16 veiculos.csv
```

<br>

Logo copio, para dentro do diretório que criamos no HDFS, os arquivos, listo os mesmos e visualizo um deles para conferir se está tudo correto.

```
[cloudera@quickstart arquivos]$ hdfs dfs -put *.csv /user/cloudera/locacao
[cloudera@quickstart arquivos]$ hdfs dfs -ls /user/cloudera/locacao
Found 4 items
-rw-r--r--   1 cloudera cloudera       2365 2022-12-15 05:31 /user/cloudera/locacao/clientes.csv
-rw-r--r--   1 cloudera cloudera        390 2022-12-15 05:31 /user/cloudera/locacao/despachantes.csv
-rw-r--r--   1 cloudera cloudera       3833 2022-12-15 05:31 /user/cloudera/locacao/locacao.csv
-rw-r--r--   1 cloudera cloudera       1939 2022-12-15 05:31 /user/cloudera/locacao/veiculos.csv
[cloudera@quickstart arquivos]$ hdfs dfs -cat /user/cloudera/locacao/clientes.csv
1,16528447080,44971318961,2021-09-30,Alvito Espargosa,2019-01-25,1973-03-15,51390834589,Ativo
2,83327244065,44976832700,2028-02-24,Blasco Canto,2019-01-15,1969-10-31,51922375520,Ativo
3,54513828080,27672609233,2024-12-11,Carminda Veríssimo,2019-04-09,1967-11-03,51738662107,Ativo
4,54075418316,75523770439,2025-08-02,Cleusa Lamego,2019-05-26,1961-10-26,51824283267,Ativo
5,41148381120,55079718160,2020-08-21,Clóvis Carrasco,2019-01-28,1974-11-02,51056509510,Ativo
6,85588747194,30471250272,2020-06-05,Cátia Fróis,2019-05-23,1965-07-08,51901121962,Ativo
7,10003401804,45948223160,2027-05-27,Emílio Faro,2019-04-17,1966-09-23,51608924744,Ativo
8,41555470319,15942100562,2026-05-16,Hipólito Granja,2019-05-13,1974-09-21,51020081027,Ativo
9,86405032817,43316295637,2027-11-25,Iara Cardoso,2019-04-04,1981-06-18,51815002282,Ativo
10,15516586747,76742255766,2021-12-25,Isaura Farias,2019-05-04,1971-04-22,51081565935,Ativo
11,44818765309,63408388553,2027-05-03,Iuri Alancastre,2019-02-18,1970-07-15,51505707400,Ativo
12,11257675109,41824436778,2020-05-22,Laura Marcondes,2019-06-30,1985-08-29,51532760959,Ativo
13,88357618405,49548848905,2021-03-25,Micael Mangueira,2019-05-24,1994-01-08,51050644140,Ativo
14,58831402110,84776516129,2026-03-08,Márcio Taveira,2019-02-13,1963-06-07,51545396437,Ativo
15,86405301141,89650561775,2027-05-06,Noêmia   Tupinambá,2019-06-28,1987-03-12,51073079084,Ativo
16,70686300220,95360639928,2028-07-19,Paula Padilha,2019-04-09,1996-05-16,51468982948,Ativo
17,70805566600,61460867546,2022-05-31,Rebeca Torcuato,2019-02-09,1994-12-06,51398031738,Ativo
18,84611132765,85761272593,2023-03-24,Rosana Betancour,2019-05-10,1976-06-08,51298196500,Ativo
19,72887644190,40239379806,2022-09-06,Sabino Abrantes,2019-05-27,1982-02-16,51805190959,Ativo
20,73201278572,56448355307,2025-02-02,Severino Leiria,2019-02-15,1965-08-06,51837010991,Ativo
21,35215454663,33281183954,2027-07-26,Tobias Garcés,2019-06-15,1962-08-20,51038698605,Ativo
22,77853823525,55351556215,2028-08-14,Vanderlei Açores,2019-01-18,1998-05-06,51069055089,Inativo
23,05013312205,32675365584,2021-12-15,Vicente Rosa,2019-05-14,1983-11-09,51782871990,Ativo
24,57460105538,54721641167,2028-02-03,Xisto Mendoça,2019-06-06,1969-07-27,51862632282,Ativo
25,02251571280,83497776322,2025-02-08,Zeferino Matoso,2019-05-01,1973-03-23,51230738729,Ativo
```

<br>

Em seguida entro no beeline, a ferramenta para logar no Hive, e logo no Hive. Em seguida experimento os comandos para criar databases **"create database teste;"**, o comando para mostrar os databases existentes **"show databases;"** e o de dropar databases e suas tabelas **"drop database teste cascade;"**. Por fim crio o database locacao, o que mais usaremos no decorrer do curso e o host das tabelas principais.

<br>

> Agora, em seguida, podemos identificar como o ponteiro das instruções que inseri o indicador "beeline> " e "0: jdbc:hive2://> ".

<br>

```
[cloudera@quickstart arquivos]$ beeline
Beeline version 1.1.0-cdh5.12.0 by Apache Hive
beeline> !connect jdbc:hive2://
scan complete in 4ms
Connecting to jdbc:hive2://
Enter username for jdbc:hive2://: 
Enter password for jdbc:hive2://: 
Connected to: Apache Hive (version 1.1.0-cdh5.12.0)
Driver: Hive JDBC (version 1.1.0-cdh5.12.0)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://> create database teste;
OK
No rows affected (29.651 seconds)
0: jdbc:hive2://> show databases;
OK
+----------------+--+
| database_name  |
+----------------+--+
| default        |
| teste          |
+----------------+--+
2 rows selected (3.829 seconds)
0: jdbc:hive2://> drop database teste cascade;
OK
No rows affected (1.793 seconds)
0: jdbc:hive2://> create database locacao;
OK
No rows affected (0.548 seconds)
```

<br>

Então começo a criar nossas tabelas principais **"CREATE EXTERNAL TABLE CLIENTES () row format delimited fields terminated by ',' STORED AS TEXTFILE;"**, populá-las **"LOAD DATA INPATH '/user/cloudera/locacao/clientes.csv' INTO TABLE CLIENTES;"** e visualizar se os dados foram devidamente carregados **"SELECT * FROM CLIENTES;"**.

```
0: jdbc:hive2://> CREATE EXTERNAL TABLE CLIENTES (
. . . . . . . . > idcliente int,
. . . . . . . . > cnh string,
. . . . . . . . > cpf string,
. . . . . . . . > validadecnh date,
. . . . . . . . > nome string,
. . . . . . . . > datacadastro date,
. . . . . . . . > datanascimento date,
. . . . . . . . > telefone string,
. . . . . . . . > status string)
. . . . . . . . > row format delimited fields terminated by ',' STORED AS TEXTFILE;
OK
No rows affected (1.197 seconds)
0: jdbc:hive2://> LOAD DATA INPATH '/user/cloudera/locacao/clientes.csv' INTO TABLE CLIENTES;
Loading data to table default.clientes
22/12/15 06:46:36 [Move-Thread-0]: WARN shims.HadoopShimsSecure: Unable to inherit permissions for file hdfs://quickstart.cloudera:8020/user/hive/warehouse/clientes/clientes.csv from file hdfs://quickstart.cloudera:8020/user/hive/warehouse/clientes User does not belong to supergroup
	at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.setOwnerInt(FSNamesystem.java:1882)
	at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.setOwner(FSNamesystem.java:1856)
	at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.setOwner(NameNodeRpcServer.java:661)
	at org.apache.hadoop.hdfs.server.namenode.AuthorizationProviderProxyClientProtocol.setOwner(AuthorizationProviderProxyClientProtocol.java:187)
	at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.setOwner(ClientNamenodeProtocolServerSideTranslatorPB.java:465)
	at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
	at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:617)
	at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:1073)
	at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2217)
	at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2213)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.security.auth.Subject.doAs(Subject.java:415)
	at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1917)
	at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2211)

Table default.clientes stats: [numFiles=1, totalSize=2365]
OK
No rows affected (3.431 seconds)
0: jdbc:hive2://> SELECT * FROM CLIENTES;
OK
+---------------------+---------------+---------------+-----------------------+---------------------+------------------------+--------------------------+--------------------+------------------+--+
| clientes.idcliente  | clientes.cnh  | clientes.cpf  | clientes.validadecnh  |    clientes.nome    | clientes.datecadastro  | clientes.datenascimento  | clientes.telefone  | clientes.status  |
+---------------------+---------------+---------------+-----------------------+---------------------+------------------------+--------------------------+--------------------+------------------+--+
| 1                   | 16528447080   | 44971318961   | 2021-09-30            | Alvito Espargosa    | 2019-01-25             | 1973-03-15               | 51390834589        | Ativo            |
| 2                   | 83327244065   | 44976832700   | 2028-02-24            | Blasco Canto        | 2019-01-15             | 1969-10-31               | 51922375520        | Ativo            |
| 3                   | 54513828080   | 27672609233   | 2024-12-11            | Carminda Veríssimo  | 2019-04-09             | 1967-11-03               | 51738662107        | Ativo            |
| 4                   | 54075418316   | 75523770439   | 2025-08-02            | Cleusa Lamego       | 2019-05-26             | 1961-10-26               | 51824283267        | Ativo            |
| 5                   | 41148381120   | 55079718160   | 2020-08-21            | Clóvis Carrasco     | 2019-01-28             | 1974-11-02               | 51056509510        | Ativo            |
| 6                   | 85588747194   | 30471250272   | 2020-06-05            | Cátia Fróis         | 2019-05-23             | 1965-07-08               | 51901121962        | Ativo            |
| 7                   | 10003401804   | 45948223160   | 2027-05-27            | Emílio Faro         | 2019-04-17             | 1966-09-23               | 51608924744        | Ativo            |
| 8                   | 41555470319   | 15942100562   | 2026-05-16            | Hipólito Granja     | 2019-05-13             | 1974-09-21               | 51020081027        | Ativo            |
| 9                   | 86405032817   | 43316295637   | 2027-11-25            | Iara Cardoso        | 2019-04-04             | 1981-06-18               | 51815002282        | Ativo            |
| 10                  | 15516586747   | 76742255766   | 2021-12-25            | Isaura Farias       | 2019-05-04             | 1971-04-22               | 51081565935        | Ativo            |
| 11                  | 44818765309   | 63408388553   | 2027-05-03            | Iuri Alancastre     | 2019-02-18             | 1970-07-15               | 51505707400        | Ativo            |
| 12                  | 11257675109   | 41824436778   | 2020-05-22            | Laura Marcondes     | 2019-06-30             | 1985-08-29               | 51532760959        | Ativo            |
| 13                  | 88357618405   | 49548848905   | 2021-03-25            | Micael Mangueira    | 2019-05-24             | 1994-01-08               | 51050644140        | Ativo            |
| 14                  | 58831402110   | 84776516129   | 2026-03-08            | Márcio Taveira      | 2019-02-13             | 1963-06-07               | 51545396437        | Ativo            |
| 15                  | 86405301141   | 89650561775   | 2027-05-06            | Noêmia   Tupinambá  | 2019-06-28             | 1987-03-12               | 51073079084        | Ativo            |
| 16                  | 70686300220   | 95360639928   | 2028-07-19            | Paula Padilha       | 2019-04-09             | 1996-05-16               | 51468982948        | Ativo            |
| 17                  | 70805566600   | 61460867546   | 2022-05-31            | Rebeca Torcuato     | 2019-02-09             | 1994-12-06               | 51398031738        | Ativo            |
| 18                  | 84611132765   | 85761272593   | 2023-03-24            | Rosana Betancour    | 2019-05-10             | 1976-06-08               | 51298196500        | Ativo            |
| 19                  | 72887644190   | 40239379806   | 2022-09-06            | Sabino Abrantes     | 2019-05-27             | 1982-02-16               | 51805190959        | Ativo            |
| 20                  | 73201278572   | 56448355307   | 2025-02-02            | Severino Leiria     | 2019-02-15             | 1965-08-06               | 51837010991        | Ativo            |
| 21                  | 35215454663   | 33281183954   | 2027-07-26            | Tobias Garcés       | 2019-06-15             | 1962-08-20               | 51038698605        | Ativo            |
| 22                  | 77853823525   | 55351556215   | 2028-08-14            | Vanderlei Açores    | 2019-01-18             | 1998-05-06               | 51069055089        | Inativo          |
| 23                  | 05013312205   | 32675365584   | 2021-12-15            | Vicente Rosa        | 2019-05-14             | 1983-11-09               | 51782871990        | Ativo            |
| 24                  | 57460105538   | 54721641167   | 2028-02-03            | Xisto Mendoça       | 2019-06-06             | 1969-07-27               | 51862632282        | Ativo            |
| 25                  | 02251571280   | 83497776322   | 2025-02-08            | Zeferino Matoso     | 2019-05-01             | 1973-03-23               | 51230738729        | Ativo            |
+---------------------+---------------+---------------+-----------------------+---------------------+------------------------+--------------------------+--------------------+------------------+--+
25 rows selected (
0: jdbc:hive2://> CREATE EXTERNAL TABLE VEICULOS (idveiculo int, dataaquisicao date, ano int, modelo string, placa string, status string, diaria double) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS TEXTFILE;
OK
No rows affected (1.428 seconds)
0: jdbc:hive2://> LOAD DATA INPATH '/user/cloudera/locacao/veiculos.csv' INTO TABLE VEICULOS;
Loading data to table locacao.veiculos
22/12/21 07:51:26 [Move-Thread-0]: WARN shims.HadoopShimsSecure: Unable to inherit permissions for file hdfs://quickstart.cloudera:8020/user/hive/warehouse/locacao.db/veiculos/veiculos.csv from file hdfs://quickstart.cloudera:8020/user/hive/warehouse/locacao.db/veiculos User does not belong to supergroup
	at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.setOwnerInt(FSNamesystem.java:1882)
	at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.setOwner(FSNamesystem.java:1856)
	at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.setOwner(NameNodeRpcServer.java:661)
	at org.apache.hadoop.hdfs.server.namenode.AuthorizationProviderProxyClientProtocol.setOwner(AuthorizationProviderProxyClientProtocol.java:187)
	at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.setOwner(ClientNamenodeProtocolServerSideTranslatorPB.java:465)
	at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
	at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:617)
	at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:1073)
	at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2217)
	at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2213)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.security.auth.Subject.doAs(Subject.java:415)
	at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1917)
	at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2211)

Table locacao.veiculos stats: [numFiles=1, totalSize=1939]
OK
No rows affected (0.946 seconds)
0: jdbc:hive2://> SELECT * FROM VEICULOS;
OK
+---------------------+-------------------------+---------------+------------------------------+-----------------+------------------+------------------+--+
| veiculos.idveiculo  | veiculos.dataaquisicao  | veiculos.ano  |       veiculos.modelo        | veiculos.placa  | veiculos.status  | veiculos.diaria  |
+---------------------+-------------------------+---------------+------------------------------+-----------------+------------------+------------------+--+
| 1                   | 2019-01-25              | 2019          | Tesla Model S P90D           | LWJ2929         | Disponivel       | 1800.0           |
| 2                   | 2019-01-04              | 2019          | Volvo XC90 T8 Hybrid         | LWJ2930         | Disponivel       | 1600.0           |
| 3                   | 2019-01-10              | 2019          | BMW 750i xDrive              | LWJ2931         | Disponivel       | 1450.0           |
| 4                   | 2019-01-28              | 2019          | Mercedes-Benz S65 AMG Coupe  | LWJ2932         | Disponivel       | 1950.0           |
| 5                   | 2019-01-12              | 2019          | Infiniti Q50S 3.7 Sedan      | LWJ2933         | Disponivel       | 2100.0           |
| 6                   | 2019-01-30              | 2019          | Volvo XC90 T8 Hybrid         | LWJ2934         | Disponivel       | 1600.0           |
| 7                   | 2019-01-06              | 2019          | BMW 750i xDrive              | LWJ2935         | Disponivel       | 1450.0           |
| 8                   | 2019-01-24              | 2019          | Infiniti Q50S 3.7 Sedan      | LWJ2936         | Disponivel       | 2100.0           |
| 9                   | 2019-01-27              | 2019          | BMW 750i xDrive              | LWJ2937         | Disponivel       | 1450.0           |
| 10                  | 2019-01-19              | 2019          | Tesla Model S P90D           | LWJ2938         | Disponivel       | 1800.0           |
| 11                  | 2019-01-27              | 2019          | Infiniti Q50S 3.7 Sedan      | LWJ2939         | Disponivel       | 2100.0           |
| 12                  | 2019-01-27              | 2019          | BMW 750i xDrive              | LWJ2940         | Disponivel       | 1450.0           |
| 13                  | 2019-01-21              | 2019          | Mercedes-Benz S65 AMG Coupe  | LWJ2941         | Disponivel       | 1950.0           |
| 14                  | 2019-01-10              | 2019          | Infiniti Q50S 3.7 Sedan      | LWJ2942         | Disponivel       | 2100.0           |
| 15                  | 2019-01-07              | 2019          | Volvo XC90 T8 Hybrid         | LWJ2943         | Disponivel       | 1600.0           |
| 16                  | 2019-01-25              | 2019          | Volvo XC90 T8 Hybrid         | LWJ2944         | Disponivel       | 1600.0           |
| 17                  | 2019-01-22              | 2019          | Volvo XC90 T8 Hybrid         | LWJ2945         | Disponivel       | 1600.0           |
| 18                  | 2019-01-24              | 2019          | BMW 750i xDrive              | LWJ2946         | Disponivel       | 1450.0           |
| 19                  | 2019-01-16              | 2019          | Mercedes-Benz S65 AMG Coupe  | LWJ2947         | Disponivel       | 1950.0           |
| 20                  | 2019-01-30              | 2019          | Volvo XC90 T8 Hybrid         | LWJ2948         | Disponivel       | 1600.0           |
| 21                  | 2019-01-04              | 2019          | Tesla Model S P90D           | LWJ2949         | Disponivel       | 1800.0           |
| 22                  | 2019-01-23              | 2019          | Infiniti Q50S 3.7 Sedan      | LWJ2950         | Disponivel       | 2100.0           |
| 23                  | 2019-01-13              | 2019          | BMW 750i xDrive              | LWJ2951         | Disponivel       | 1450.0           |
| 24                  | 2019-01-05              | 2019          | Volvo XC90 T8 Hybrid         | LWJ2952         | Disponivel       | 1600.0           |
| 25                  | 2019-01-06              | 2019          | Mercedes-Benz S65 AMG Coupe  | LWJ2953         | Indisponivel     | 1950.0           |
| 26                  | 2019-01-05              | 2019          | BMW 750i xDrive              | LWJ2954         | Disponivel       | 1450.0           |
| 27                  | 2019-01-30              | 2019          | Tesla Model S P90D           | LWJ2955         | Disponivel       | 1800.0           |
| 28                  | 2019-01-28              | 2019          | BMW 750i xDrive              | LWJ2956         | Disponivel       | 1450.0           |
| 29                  | 2019-01-15              | 2019          | Infiniti Q50S 3.7 Sedan      | LWJ2957         | Disponivel       | 2100.0           |
| 30                  | 2019-01-12              | 2019          | BMW 750i xDrive              | LWJ2958         | Disponivel       | 1450.0           |
+---------------------+-------------------------+---------------+------------------------------+-----------------+------------------+------------------+--+
30 rows selected (0.169 seconds)
0: jdbc:hive2://> CREATE EXTERNAL TABLE DESPACHANTES (iddespachante int, nome string, status string, filial string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS TEXTFILE;
OK
No rows affected (0.502 seconds)
0: jdbc:hive2://> LOAD DATA INPATH '/user/cloudera/locacao/despachantes.csv' INTO TABLE DESPACHANTES;
Loading data to table locacao.despachantes
22/12/21 08:42:11 [Move-Thread-0]: WARN shims.HadoopShimsSecure: Unable to inherit permissions for file hdfs://quickstart.cloudera:8020/user/hive/warehouse/locacao.db/despachantes/despachantes.csv from file hdfs://quickstart.cloudera:8020/user/hive/warehouse/locacao.db/despachantes User does not belong to supergroup
	at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.setOwnerInt(FSNamesystem.java:1882)
	at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.setOwner(FSNamesystem.java:1856)
	at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.setOwner(NameNodeRpcServer.java:661)
	at org.apache.hadoop.hdfs.server.namenode.AuthorizationProviderProxyClientProtocol.setOwner(AuthorizationProviderProxyClientProtocol.java:187)
	at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.setOwner(ClientNamenodeProtocolServerSideTranslatorPB.java:465)
	at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
	at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:617)
	at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:1073)
	at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2217)
	at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2213)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.security.auth.Subject.doAs(Subject.java:415)
	at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1917)
	at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2211)

Table locacao.despachantes stats: [numFiles=1, totalSize=390]
OK
No rows affected (2.625 seconds)
0: jdbc:hive2://> SELECT * FROM DESPACHANTES;
OK
+-----------------------------+----------------------+----------------------+----------------------+--+
| despachantes.iddespachante  |  despachantes.nome   | despachantes.status  | despachantes.filial  |
+-----------------------------+----------------------+----------------------+----------------------+--+
| 1                           | Carminda Pestana     | Ativo                | Santa Maria          |
| 2                           | Deolinda Vilela      | Ativo                | Novo Hamburgo        |
| 3                           | Emídio Dornelles     | Ativo                | Porto Alegre         |
| 4                           | Felisbela Dornelles  | Ativo                | Porto Alegre         |
| 5                           | Graça Ornellas       | Ativo                | Porto Alegre         |
| 6                           | Matilde Rebouças     | Ativo                | Porto Alegre         |
| 7                           | Noêmia   Orriça      | Ativo                | Santa Maria          |
| 8                           | Roque Vásquez        | Ativo                | Porto Alegre         |
| 9                           | Uriel Queiroz        | Ativo                | Porto Alegre         |
| 10                          | Viviana Sequeira     | Ativo                | Porto Alegre         |
+-----------------------------+----------------------+----------------------+----------------------+--+
10 rows selected (0.575 seconds)
0: jdbc:hive2://> CREATE EXTERNAL TABLE LOCACAO (idlocacao int, idcliente int, iddespachante int, idveiculo int, datalocacao date, dataentrega date, total double) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS TEXTFILE;
OK
No rows affected (0.539 seconds)
0: jdbc:hive2://> LOAD DATA INPATH '/user/cloudera/locacao/locacao.csv' INTO TABLE LOCACAO;
Loading data to table locacao.locacao
22/12/21 08:45:37 [Move-Thread-0]: WARN shims.HadoopShimsSecure: Unable to inherit permissions for file hdfs://quickstart.cloudera:8020/user/hive/warehouse/locacao.db/locacao/locacao.csv from file hdfs://quickstart.cloudera:8020/user/hive/warehouse/locacao.db/locacao User does not belong to supergroup
	at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.setOwnerInt(FSNamesystem.java:1882)
	at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.setOwner(FSNamesystem.java:1856)
	at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.setOwner(NameNodeRpcServer.java:661)
	at org.apache.hadoop.hdfs.server.namenode.AuthorizationProviderProxyClientProtocol.setOwner(AuthorizationProviderProxyClientProtocol.java:187)
	at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.setOwner(ClientNamenodeProtocolServerSideTranslatorPB.java:465)
	at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
	at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:617)
	at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:1073)
	at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2217)
	at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2213)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.security.auth.Subject.doAs(Subject.java:415)
	at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1917)
	at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2211)

Table locacao.locacao stats: [numFiles=1, totalSize=3833]
OK
No rows affected (1.205 seconds)
0: jdbc:hive2://> SELECT * FROM LOCACAO;
OK
+--------------------+--------------------+------------------------+--------------------+----------------------+----------------------+----------------+--+
| locacao.idlocacao  | locacao.idcliente  | locacao.iddespachante  | locacao.idveiculo  | locacao.datalocacao  | locacao.dataentrega  | locacao.total  |
+--------------------+--------------------+------------------------+--------------------+----------------------+----------------------+----------------+--+
| 1                  | 3                  | 5                      | 22                 | 2019-02-20           | 2019-02-20           | 1996.0         |
| 2                  | 5                  | 5                      | 7                  | 2019-06-30           | 2019-07-03           | 1843.0         |
| 3                  | 17                 | 5                      | 3                  | 2019-03-09           | 2019-03-11           | 2016.0         |
| 4                  | 24                 | 3                      | 6                  | 2019-04-19           | 2019-04-22           | 1857.0         |
| 5                  | 8                  | 10                     | 3                  | 2019-02-24           | 2019-02-26           | 2049.0         |
| 6                  | 14                 | 5                      | 23                 | 2019-05-15           | 2019-05-19           | 1986.0         |
| 7                  | 13                 | 2                      | 6                  | 2019-05-01           | 2019-05-04           | 1989.0         |
| 8                  | 22                 | 5                      | 29                 | 2019-03-29           | 2019-04-03           | 1874.0         |
| 9                  | 17                 | 7                      | 13                 | 2019-05-10           | 2019-05-13           | 2027.0         |
| 10                 | 1                  | 2                      | 21                 | 2019-03-17           | 2019-03-19           | 2010.0         |
| 11                 | 11                 | 5                      | 19                 | 2019-05-12           | 2019-05-17           | 2006.0         |
| 12                 | 19                 | 10                     | 2                  | 2019-02-26           | 2019-02-26           | 1915.0         |
| 13                 | 20                 | 6                      | 9                  | 2019-06-12           | 2019-06-17           | 2089.0         |
| 14                 | 24                 | 5                      | 3                  | 2019-06-04           | 2019-06-06           | 1942.0         |
| 15                 | 10                 | 10                     | 1                  | 2019-02-22           | 2019-02-23           | 2071.0         |
| 16                 | 17                 | 4                      | 13                 | 2019-05-29           | 2019-05-29           | 2035.0         |
| 17                 | 4                  | 1                      | 11                 | 2019-04-09           | 2019-04-13           | 1810.0         |
| 18                 | 9                  | 8                      | 11                 | 2019-04-21           | 2019-04-23           | 2017.0         |
| 19                 | 3                  | 7                      | 29                 | 2019-05-11           | 2019-05-15           | 2095.0         |
| 20                 | 1                  | 10                     | 2                  | 2019-06-17           | 2019-06-17           | 2020.0         |
| 21                 | 13                 | 6                      | 1                  | 2019-04-12           | 2019-04-15           | 2077.0         |
| 22                 | 16                 | 6                      | 8                  | 2019-06-02           | 2019-06-03           | 1867.0         |
| 23                 | 19                 | 9                      | 17                 | 2019-05-12           | 2019-05-17           | 1862.0         |
| 24                 | 13                 | 6                      | 20                 | 2019-04-06           | 2019-04-07           | 1855.0         |
| 25                 | 21                 | 4                      | 21                 | 2019-04-26           | 2019-04-28           | 2033.0         |
| 26                 | 25                 | 10                     | 8                  | 2019-03-16           | 2019-03-21           | 2034.0         |
| 27                 | 8                  | 2                      | 3                  | 2019-05-06           | 2019-05-09           | 1865.0         |
| 28                 | 15                 | 3                      | 1                  | 2019-06-05           | 2019-06-07           | 1997.0         |
| 29                 | 22                 | 7                      | 8                  | 2019-05-07           | 2019-05-12           | 2032.0         |
| 30                 | 13                 | 3                      | 7                  | 2019-05-06           | 2019-05-06           | 2011.0         |
| 31                 | 14                 | 1                      | 21                 | 2019-03-26           | 2019-03-28           | 1867.0         |
| 32                 | 19                 | 9                      | 13                 | 2019-03-14           | 2019-03-18           | 1963.0         |
| 33                 | 6                  | 1                      | 20                 | 2019-04-15           | 2019-04-16           | 1845.0         |
| 34                 | 13                 | 2                      | 26                 | 2019-06-28           | 2019-07-02           | 2020.0         |
| 35                 | 15                 | 4                      | 2                  | 2019-06-22           | 2019-06-25           | 1938.0         |
| 36                 | 5                  | 1                      | 18                 | 2019-06-01           | 2019-06-06           | 1980.0         |
| 37                 | 11                 | 4                      | 20                 | 2019-03-21           | 2019-03-23           | 2037.0         |
| 38                 | 24                 | 6                      | 23                 | 2019-05-31           | 2019-06-01           | 1970.0         |
| 39                 | 7                  | 10                     | 18                 | 2019-05-02           | 2019-05-03           | 1900.0         |
| 40                 | 21                 | 1                      | 9                  | 2019-06-15           | 2019-06-19           | 1888.0         |
| 41                 | 17                 | 10                     | 4                  | 2019-02-20           | 2019-02-25           | 2042.0         |
| 42                 | 9                  | 1                      | 5                  | 2019-03-04           | 2019-03-04           | 1821.0         |
| 43                 | 4                  | 3                      | 10                 | 2019-05-21           | 2019-05-24           | 2009.0         |
| 44                 | 20                 | 1                      | 14                 | 2019-04-02           | 2019-04-02           | 1837.0         |
| 45                 | 13                 | 9                      | 21                 | 2019-06-08           | 2019-06-10           | 1971.0         |
| 46                 | 6                  | 3                      | 21                 | 2019-05-28           | 2019-05-30           | 1818.0         |
| 47                 | 15                 | 5                      | 6                  | 2019-02-22           | 2019-02-22           | 2031.0         |
| 48                 | 9                  | 7                      | 8                  | 2019-06-04           | 2019-06-09           | 1938.0         |
| 49                 | 1                  | 2                      | 22                 | 2019-03-08           | 2019-03-10           | 1975.0         |
| 50                 | 23                 | 10                     | 1                  | 2019-02-27           | 2019-03-04           | 2014.0         |
| 51                 | 17                 | 10                     | 15                 | 2019-03-15           | 2019-03-19           | 1958.0         |
| 52                 | 21                 | 10                     | 1                  | 2019-05-11           | 2019-05-11           | 2024.0         |
| 53                 | 12                 | 1                      | 20                 | 2019-04-20           | 2019-04-20           | 1946.0         |
| 54                 | 23                 | 6                      | 22                 | 2019-05-07           | 2019-05-09           | 1855.0         |
| 55                 | 13                 | 5                      | 15                 | 2019-03-05           | 2019-03-09           | 2039.0         |
| 56                 | 16                 | 3                      | 28                 | 2019-03-18           | 2019-03-21           | 1991.0         |
| 57                 | 3                  | 5                      | 5                  | 2019-03-18           | 2019-03-18           | 1914.0         |
| 58                 | 7                  | 5                      | 28                 | 2019-04-21           | 2019-04-22           | 1890.0         |
| 59                 | 23                 | 7                      | 24                 | 2019-06-21           | 2019-06-24           | 1840.0         |
| 60                 | 15                 | 1                      | 4                  | 2019-03-27           | 2019-03-28           | 2004.0         |
| 61                 | 9                  | 2                      | 20                 | 2019-03-08           | 2019-03-08           | 2075.0         |
| 62                 | 23                 | 5                      | 2                  | 2019-05-10           | 2019-05-11           | 1838.0         |
| 63                 | 5                  | 9                      | 30                 | 2019-05-21           | 2019-05-22           | 1974.0         |
| 64                 | 23                 | 4                      | 8                  | 2019-02-26           | 2019-03-02           | 2064.0         |
| 65                 | 22                 | 8                      | 18                 | 2019-05-22           | 2019-05-22           | 1836.0         |
| 66                 | 11                 | 4                      | 17                 | 2019-04-20           | 2019-04-20           | 1800.0         |
| 67                 | 2                  | 7                      | 7                  | 2019-06-22           | 2019-06-25           | 2070.0         |
| 68                 | 4                  | 5                      | 26                 | 2019-04-11           | 2019-04-14           | 2001.0         |
| 69                 | 11                 | 6                      | 20                 | 2019-02-20           | 2019-02-21           | 1858.0         |
| 70                 | 16                 | 4                      | 11                 | 2019-05-26           | 2019-05-26           | 1983.0         |
| 71                 | 6                  | 1                      | 9                  | 2019-06-08           | 2019-06-08           | 1853.0         |
| 72                 | 7                  | 2                      | 25                 | 2019-06-14           | 2019-06-14           | 1813.0         |
| 73                 | 11                 | 7                      | 19                 | 2019-03-27           | 2019-03-31           | 1950.0         |
| 74                 | 6                  | 5                      | 24                 | 2019-06-23           | 2019-06-23           | 1831.0         |
| 75                 | 20                 | 9                      | 13                 | 2019-06-03           | 2019-06-07           | 2049.0         |
| 76                 | 7                  | 8                      | 30                 | 2019-05-13           | 2019-05-17           | 1843.0         |
| 77                 | 24                 | 1                      | 30                 | 2019-06-26           | 2019-06-30           | 1881.0         |
| 78                 | 18                 | 5                      | 25                 | 2019-05-19           | 2019-05-24           | 1810.0         |
| 79                 | 7                  | 7                      | 5                  | 2019-04-28           | 2019-05-02           | 1899.0         |
| 80                 | 16                 | 1                      | 9                  | 2019-06-15           | 2019-06-17           | 1812.0         |
| 81                 | 22                 | 2                      | 22                 | 2019-04-08           | 2019-04-13           | 1921.0         |
| 82                 | 9                  | 6                      | 24                 | 2019-04-03           | 2019-04-03           | 1945.0         |
| 83                 | 11                 | 6                      | 2                  | 2019-04-01           | 2019-04-05           | 1990.0         |
| 84                 | 6                  | 8                      | 25                 | 2019-04-16           | 2019-04-18           | 1956.0         |
| 85                 | 23                 | 10                     | 15                 | 2019-04-05           | 2019-04-05           | 1927.0         |
| 86                 | 23                 | 6                      | 4                  | 2019-04-23           | 2019-04-27           | 1801.0         |
| 87                 | 11                 | 6                      | 18                 | 2019-03-22           | 2019-03-22           | 1802.0         |
| 88                 | 9                  | 5                      | 17                 | 2019-02-27           | 2019-02-27           | 1851.0         |
| 89                 | 16                 | 4                      | 24                 | 2019-06-22           | 2019-06-22           | 2089.0         |
| 90                 | 7                  | 5                      | 25                 | 2019-03-27           | 2019-03-30           | 1967.0         |
| 91                 | 21                 | 1                      | 9                  | 2019-05-07           | 2019-05-12           | 1826.0         |
| 92                 | 24                 | 9                      | 30                 | 2019-05-13           | 2019-05-13           | 1864.0         |
| 93                 | 13                 | 5                      | 12                 | 2019-03-10           | 2019-03-13           | 2056.0         |
| 94                 | 18                 | 6                      | 4                  | 2019-05-22           | 2019-05-25           | 1986.0         |
| 95                 | 22                 | 2                      | 28                 | 2019-02-21           | 2019-02-21           | 1861.0         |
| 96                 | 5                  | 10                     | 17                 | 2019-06-22           | 2019-06-26           | 1860.0         |
| 97                 | 16                 | 9                      | 1                  | 2019-02-27           | 2019-03-03           | 2100.0         |
| 98                 | 8                  | 3                      | 2                  | 2019-05-29           | 2019-05-29           | 2054.0         |
| 99                 | 1                  | 3                      | 30                 | 2019-03-30           | 2019-04-01           | 1886.0         |
| 100                | 15                 | 8                      | 20                 | 2019-06-12           | 2019-06-14           | 1939.0         |
+--------------------+--------------------+------------------------+--------------------+----------------------+----------------------+----------------+--+
100 rows selected (0.621 seconds)
0: jdbc:hive2://> 
```

<br>
<br>

Finalizo a descrição desse script aqui. Acrescento que são minhas observações durante meu aprendizado e que ainda estou no início, então peço paciência sobre qualquer erro (= Obrigada pela compreensão :smile:

Obrigada por ler até o final! :wink:




