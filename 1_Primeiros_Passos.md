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

Então confirmado meu acesso à pasta, em seguida listei os arquivos e pastas presentes no HDFS, entrei no diretório de user cloudera e criei o diretório "locacao" para que no futuro possa copiar os arquivos das bases de dados para dentro do mesmo.

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
