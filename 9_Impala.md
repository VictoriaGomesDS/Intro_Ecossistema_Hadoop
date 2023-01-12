# Explicando o desenvolvimento do Script "Impala" :pencil:

Esse foi o script onde mostro a conexão no **Impala** e um pouco da execução de querys no mesmo.

<br>

> Podemos identificar como os comandos que inseri aquelas instruções que estão a frente do indicador "[quickstart.cloudera:21000] ".

<br>

Primeiramente, eu conecto ao **Impala-shell**.

```
[cloudera@quickstart ~]$ impala-shell
Starting Impala Shell without Kerberos authentication
Connected to quickstart.cloudera:21000
Server version: impalad version 2.9.0-cdh5.12.0 RELEASE (build 03c6ddbdcec39238be4f5b14a300d5c4f576097e)
***********************************************************************************
Welcome to the Impala shell.
(Impala Shell v2.9.0-cdh5.12.0 (03c6ddb) built on Thu Jun 29 04:17:31 PDT 2017)

To see live updates on a query's progress, run 'set LIVE_SUMMARY=1;'.
***********************************************************************************
```

<br>
<br>

Logo, eu faço um **SHOW DATABASES;** e vejo se a as base de dados estão já carregadas na interface do Impala. 
Mesmo aparecendo os databases, por garantia, faço um **"INVALIDATE METADATA;"**.

```
[quickstart.cloudera:21000] > SHOW DATABASES;
Query: show DATABASES
+------------------+----------------------------------------------+
| name             | comment                                      |
+------------------+----------------------------------------------+
| _impala_builtins | System database for Impala builtin functions |
| default          | Default Hive database                        |
| locacao          |                                              |
| retail_db        |                                              |
+------------------+----------------------------------------------+
Fetched 4 row(s) in 1.30s
[quickstart.cloudera:21000] > INVALIDATE METADATA;
Query: invalidate METADATA
Query submitted at: 2023-01-10 06:28:48 (Coordinator: http://quickstart.cloudera:25000)
Query progress can be monitored at: http://quickstart.cloudera:25000/query_plan?query_id=9d417e6436ccfbad:b552acb500000000

Fetched 0 row(s) in 3.88s
```

<br>
<br>

Aqui, já inserida no database locacao, eu faço uma query **SELECT * FROM locacao;** que gera um erro proposital, para que eu mostrasse uma das incompatibilidades do Impala e do Hue: 

<br>

> O Impala não suporta o tipo ***date*** do Hive, apenas ***timestamp***, então é gerado o erro: **"Unsupported type 'DATE' in 'locacao.locacao.datalocacao'."** - pois um dos campos ta tabela locacao é date.

<br>

```
[quickstart.cloudera:21000] > USE locacao;
Query: use locacao
[quickstart.cloudera:21000] > SHOW TABLES;
Query: show TABLES
+-------------------+
| name              |
+-------------------+
| clientes          |
| clientes_orc      |
| despachantes      |
| locacao           |
| locacao_orc       |
| locacaoanalitico  |
| locacaoanalitico2 |
| veiculos          |
+-------------------+
Fetched 8 row(s) in 0.03s
[quickstart.cloudera:21000] > SELECT * FROM locacao;
Query: select * FROM locacao
Query submitted at: 2023-01-10 06:30:00 (Coordinator: http://quickstart.cloudera:25000)
ERROR: AnalysisException: Unsupported type 'DATE' in 'locacao.locacao.datalocacao'.

```

<br>
<br>

Aqui faço duas querys, eu faço uma selecionando a soma dos valores da coluna total da tabela locacao: **SELECT SUM(total) FROM locacao;**.
E faço outra selecionando o maior valor da coluna total da tabela locacao: **SELECT MAX(total) FROM locacao;**.

```
[quickstart.cloudera:21000] > SELECT SUM(total) FROM locacao;
Query: select SUM(total) FROM locacao
Query submitted at: 2023-01-10 06:36:32 (Coordinator: http://quickstart.cloudera:25000)
Query progress can be monitored at: http://quickstart.cloudera:25000/query_plan?query_id=9840050bac9b4616:766d6f8500000000
+------------+
| sum(total) |
+------------+
| 194526     |
+------------+
Fetched 1 row(s) in 7.53s
[quickstart.cloudera:21000] > SELECT MAX(total) FROM locacao;
Query: select MAX(total) FROM locacao
Query submitted at: 2023-01-10 06:40:44 (Coordinator: http://quickstart.cloudera:25000)
Query progress can be monitored at: http://quickstart.cloudera:25000/query_plan?query_id=a0456ebb7a3f8ced:a0382fa300000000
+------------+
| max(total) |
+------------+
| 2100       |
+------------+
Fetched 1 row(s) in 2.16s
```

<br>
<br>

Aqui faço três querys. Uma selecionando duas coluna total da tabela veiculos, onde modelo o modelo é da marca BMW: **SELECT idveiculo, modelo FROM veiculos WHERE modelo LIKE 'BMW%';**.
E faço outra selecionando todos os dados da tabela despachantes, onde a filial seja em Santa Maria ou Novo Hamburgo: **SELECT * FROM despachantes WHERE filial IN ('Santa Maria','Novo Hamburgo');**.
E, na última, faço uma query selecionando dotos os dados da tabela despachantes, onde a filial não seja em Santa Maria ou Novo Hamburgo: **SELECT * FROM despachantes WHERE filial NOT IN ('Santa Maria','Novo Hamburgo');**.

```
[quickstart.cloudera:21000] > SELECT idveiculo, modelo FROM veiculos WHERE modelo LIKE 'BMW%';
Query: select idveiculo, modelo FROM veiculos WHERE modelo LIKE 'BMW%'
Query submitted at: 2023-01-10 06:42:25 (Coordinator: http://quickstart.cloudera:25000)
Query progress can be monitored at: http://quickstart.cloudera:25000/query_plan?query_id=2a408ada39daf436:a2efc29900000000
+-----------+-----------------+
| idveiculo | modelo          |
+-----------+-----------------+
| 3         | BMW 750i xDrive |
| 7         | BMW 750i xDrive |
| 9         | BMW 750i xDrive |
| 12        | BMW 750i xDrive |
| 18        | BMW 750i xDrive |
| 23        | BMW 750i xDrive |
| 26        | BMW 750i xDrive |
| 28        | BMW 750i xDrive |
| 30        | BMW 750i xDrive |
+-----------+-----------------+
Fetched 9 row(s) in 2.50s
[quickstart.cloudera:21000] > SELECT * FROM despachantes WHERE filial IN ('Santa Maria','Novo Hamburgo');
Query: select * FROM despachantes WHERE filial IN ('Santa Maria','Novo Hamburgo')
Query submitted at: 2023-01-10 06:44:22 (Coordinator: http://quickstart.cloudera:25000)
Query progress can be monitored at: http://quickstart.cloudera:25000/query_plan?query_id=2e4cbcac8b5e31e0:dc6d014b00000000
+---------------+------------------+--------+---------------+
| iddespachante | nome             | status | filial        |
+---------------+------------------+--------+---------------+
| NULL          | Carminda Pestana | Ativo  | Santa Maria   |
| 2             | Deolinda Vilela  | Ativo  | Novo Hamburgo |
| 7             | Noêmia   Orriça  | Ativo  | Santa Maria   |
+---------------+------------------+--------+---------------+
WARNINGS: Error converting column: 0 to INT
Error parsing row: file: hdfs://quickstart.cloudera:8020/user/hive/warehouse/locacao.db/despachantes/despachantes.csv, before offset: 390

Fetched 3 row(s) in 1.72s
[quickstart.cloudera:21000] > SELECT * FROM despachantes WHERE filial NOT IN ('Santa Maria','Novo Hamburgo');
Query: select * FROM despachantes WHERE filial NOT IN ('Santa Maria','Novo Hamburgo')
Query submitted at: 2023-01-10 06:45:06 (Coordinator: http://quickstart.cloudera:25000)
Query progress can be monitored at: http://quickstart.cloudera:25000/query_plan?query_id=9c4e345798b4b38c:75f9545a00000000
+---------------+---------------------+--------+--------------+
| iddespachante | nome                | status | filial       |
+---------------+---------------------+--------+--------------+
| 3             | Emídio Dornelles    | Ativo  | Porto Alegre |
| 4             | Felisbela Dornelles | Ativo  | Porto Alegre |
| 5             | Graça Ornellas      | Ativo  | Porto Alegre |
| 6             | Matilde Rebouças    | Ativo  | Porto Alegre |
| 8             | Roque Vásquez       | Ativo  | Porto Alegre |
| 9             | Uriel Queiroz       | Ativo  | Porto Alegre |
| 10            | Viviana Sequeira    | Ativo  | Porto Alegre |
+---------------+---------------------+--------+--------------+
Fetched 7 row(s) in 0.28s
```

<br>
<br>

Aqui, por último, trago uma query que seleciona o limite de 10 linhas de diferentes colunas da tabela locacao com um join de despachantes.

```
[quickstart.cloudera:21000] > SELECT loc.total, des.nome FROM locacao loc
                            > JOIN despachantes des ON (loc.iddespachante = des.iddespachante)
                            > LIMIT 10;
Query: select loc.total, des.nome FROM locacao loc
JOIN despachantes des ON (loc.iddespachante = des.iddespachante)
LIMIT 10
Query submitted at: 2023-01-10 06:47:19 (Coordinator: http://quickstart.cloudera:25000)
Query progress can be monitored at: http://quickstart.cloudera:25000/query_plan?query_id=e749e3472e53a4ca:44912f1d00000000
+-------+------------------+
| total | nome             |
+-------+------------------+
| 1996  | Graça Ornellas   |
| 1843  | Graça Ornellas   |
| 2016  | Graça Ornellas   |
| 1857  | Emídio Dornelles |
| 2049  | Viviana Sequeira |
| 1986  | Graça Ornellas   |
| 1989  | Deolinda Vilela  |
| 1874  | Graça Ornellas   |
| 2027  | Noêmia   Orriça  |
| 2010  | Deolinda Vilela  |
+-------+------------------+
WARNINGS: Error converting column: 0 to INT
Error parsing row: file: hdfs://quickstart.cloudera:8020/user/hive/warehouse/locacao.db/despachantes/despachantes.csv, before offset: 390

Fetched 10 row(s) in 0.65s
```


<br>
<br>

Finalizo a descrição desse script aqui. Acrescento que são minhas observações durante meu aprendizado e que ainda estou no início, então peço paciência sobre qualquer erro (= Obrigada pela compreensão :smile:

Obrigada por ler até o final! :wink:





