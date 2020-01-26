
<img src="../img/restore.png" alt="" width="80%">

# Restaurando um backup físico em uma nova instância e em um mesmo servidor de produção<br>
##### Publicado em 26/01/2020 por [Michel Souza](https://www.linkedin.com/in/michel-ferreira-souza/)

Fala galera, uma atividade que precisa de bastante atenção na vida do DBA é a restauração do backup em uma instância no mesmo servidor do ambiente produtivo. Um dos maiores receio dos **DBA's** é sobrescrever algum arquivo do ambiente produtivo ou ambiente de origem do backup.  <br>
Esse tipo de atividade tem se tornado frenquente no meu dia a dia, tanto restaurando um backup ou realizando clonagem de base para ambiente de teste.

> *"A melhor forma de aprender é ensinando ou compartilhando conhecimento."*

Neste artigo apresento como restaurar um backup realizado da base DBTESTE no artigo [Parte 4](https://github.com/souzacristsf/article_database/blob/master/ORACLE/TESTE_DBA/test_solved_parte-4.md#realizando-um-backup-f%C3%ADsico-da-inst%C3%A2ncia-de-dbteste-com-rman) na instância **DBTREINA** no mesmo servidor de origem do backup. <br>

# Ambiente
Estou utilizando as seguintes versões do banco de dados e S.O.
```
Sistema Operacional : Oracle Linux 7.6 64 Bits
Database Version    : Oracle Enterprise 19C
```
Primeiro, vamos verificar quantas instâncias existem no servidor que será realizado o restore.
```bash
[oracle@lab-ol8-19c => (dbteste ~]$ ps -ef |grep pmon
oracle    2329     1  0 14:53 ?        00:00:01 ora_pmon_cdbprd
oracle    2661     1  0 14:54 ?        00:00:01 ora_pmon_dbteste
```
Visto que a instância DBTREINA não existe, vamos criar um PFILE e SPFILE com os parâmetros necessários para a nova instância.
```bash
echo "db_name=dbteste" > $ORACLE_HOME/dbs/initdbtreina.ora
echo "db_unique_name=dbtreina" >> $ORACLE_HOME/dbs/initdbtreina.ora
echo "control_files='/u01/oradata/dbtreina/control01.ctl', '/u02/oradata/dbtreina/control02.ctl'" >> $ORACLE_HOME/dbs/initdbtreina.ora
```
> **Observação**: nota-se que será criado uma nova instância non-cdb, caso o backup fosse restaurado de uma instância CDB, é necessário informar o parâmentro **enable_pluggable_database=true**.

O comando acima cria um arquivo *initdbtreina.ora* no ORACLE_HOME/dbs, é importante que o parâmetro ```db_name``` seja setado com o nome do banco de dados de origem do backup, que nesse caso é dbteste, outro ponto a se observar é o parâmetro ```control_files``` que está setando dois destino para o controlfile. 

> **Observação**: nota-se que não está sendo utilizado o parâmetro **db_create_file_dest** ou (OMF) Oracle Managed Files, responsável por gerencia a criação e a exclusão de arquivos no nível do sistema operacional, leia mais sobre OMF [aqui](http://www.dba-oracle.com/real_application_clusters_rac_grid/omf.html) e as vantagens.

É necessário criar os diretórios abaixo, visto que não estamos utilizando OMF.
```bash
mkdir -p /u01/oradata/dbtreina/
mkdir -p /u02/oradata/dbtreina/
```

Verificando o conteúdo do arquivo ```$ORACLE_HOME/dbs/initdbtreina.ora```.
```bash
[oracle@lab-ol8-19c => (dbteste ~]$ cat  $ORACLE_HOME/dbs/initdbtreina.ora

db_name=dbteste
db_unique_name=dbtreina
control_files='/u01/oradata/dbtreina/control01.ctl', '/u02/oradata/dbtreina/control02.ctl'
```

Registrando a nova instância no arquivo ```/etc/oratab``` com o comando abaixo.
```bash
echo "dbtreina:/u01/app/oracle/product/19.3.0.0/dbhome_1:Y" >> /etc/oratab
```

Setando ORACLE_BASE, ORACLE_HOME e ORACLE_SID com a nova instância.
```bash
[oracle@lab-ol8-19c => (dbteste ~]$ . oraenv
ORACLE_SID = [dbteste] ? dbtreina
The Oracle base remains unchanged with value /u01/app/oracle
```

Agora precisamos criar um SPFILE a partir do PFILE criado e subir a instância DBTREINA em **nomount**. 
```sql
[oracle@lab-ol8-19c => (dbtreina ~]$ sqlplus / as sysdba

SQL*Plus: Release 19.0.0.0.0 - Production on Sat Jan 25 20:50:18 2020
Version 19.3.0.0.0

Copyright (c) 1982, 2019, Oracle.  All rights reserved.

Connected to an idle instance.

SQL> create spfile from pfile;

File created.

SQL> startup nomount
ORACLE instance started.

Total System Global Area  243268216 bytes
Fixed Size                  8895096 bytes
Variable Size             180355072 bytes
Database Buffers           50331648 bytes
Redo Buffers                3686400 bytes
```
Veja que no comando abaixo, tem-se o nome do banco como dbteste.
```sql
SQL> show parameter name

NAME                 TYPE        VALUE
-------------------- ----------- -------------
...
db_name              string      dbteste
db_unique_name       string      dbtreina
global_names         boolean     FALSE
instance_name        string      dbtreina
service_names        string      dbtreina
...
```

Verificando o destino do backup. 
```bash
[oracle@lab-ol8-19c => (dbtreina ~]$ ls -ltrh /u02/backup/dbteste/fisico/files
total 583M
-rw-r-----. 1 oracle oinstall 332M Jan 25 15:43 df_DBTESTE_6_1_1030635610.dbf
-rw-r-----. 1 oracle oinstall 250M Jan 25 15:46 arch_DBTESTE_8_1_1030635866.arc
-rw-r-----. 1 oracle oinstall 1.1M Jan 25 15:46 cf_DBTESTE_10_1_1030636015.ctl
-rw-r-----. 1 oracle oinstall 112K Jan 25 15:47 sp_DBTESTE_12_1_1030636024.ora
```

Realizando o restore do backup controlfile para o restore do backup.
```sql
[oracle@lab-ol8-19c => (dbtreina ~]$ rman target /

Recovery Manager: Release 19.0.0.0.0 - Production on Sat Jan 25 21:13:17 2020
Version 19.3.0.0.0

Copyright (c) 1982, 2019, Oracle and/or its affiliates.  All rights reserved.

connected to target database: DBTESTE (not mounted)

RMAN> restore controlfile from '/u02/backup/dbteste/fisico/files/cf_DBTESTE_10_1_1030636015.ctl';

Starting restore at 25/01/2020 21:13:31
using target database control file instead of recovery catalog
allocated channel: ORA_DISK_1
channel ORA_DISK_1: SID=38 device type=DISK

channel ORA_DISK_1: restoring control file
channel ORA_DISK_1: restore complete, elapsed time: 00:00:03
output file name=/u01/oradata/dbtreina/control01.ctl
output file name=/u02/oradata/dbtreina/control02.ctl
Finished restore at 25/01/2020 21:13:36

RMAN>
```
Veja que o comando **restore controlfile**, restaurou o controlfile nos destinos ```/u01/oradata/dbtreina/``` e ```/u02/oradata/dbtreina/```.

Iniciando a instância em mount com o controlfile restaurado. 
```sql
SQL> startup mount force
ORACLE instance started.

Total System Global Area  243268216 bytes
Fixed Size                  8895096 bytes
Variable Size             180355072 bytes
Database Buffers           50331648 bytes
Redo Buffers                3686400 bytes
Database mounted.
SQL>

SQL> select instance_name, status, VERSION, HOST_NAME from v$instance;

INSTANCE_NAME    STATUS       VERSION           HOST_NAME
---------------- ------------ ----------------- ---------------------------
dbtreina         MOUNTED      19.0.0.0.0        lab-ol8-19c.localdomain
```

Como não estamos utilizando OMF para gerenciar o destino dos datafiles no momento do restore, é necessário identificar os datafiles existentes, pra isso pode-se utilizar o comando ```report schema```.
```sql
[oracle@lab-ol8-19c => (dbtreina ~]$ rman target /

Recovery Manager: Release 19.0.0.0.0 - Production on Sat Jan 25 21:21:19 2020
Version 19.3.0.0.0

Copyright (c) 1982, 2019, Oracle and/or its affiliates.  All rights reserved.

connected to target database: DBTESTE (DBID=938280311, not open)

RMAN> report schema;

using target database control file instead of recovery catalog
RMAN-06139: warning: control file is not current for REPORT SCHEMA
Report of database schema for database with db_unique_name DBTREINA

List of Permanent Datafiles
===========================
File Size(MB) Tablespace           RB segs Datafile Name
---- -------- -------------------- ------- ------------------------
1    920      SYSTEM               ***     /u01/oradata/dbteste/system01.dbf
2    100      GOSALES_TSRECO       ***     /u01/oradata/dbteste/gosalesreco_data01.dbf
3    680      SYSAUX               ***     /u01/oradata/dbteste/sysaux01.dbf
4    275      UNDOTBS1             ***     /u01/oradata/dbteste/undotbs01.dbf
5    10240    GOSALES_TS           ***     /u01/oradata/dbteste/gosales_data01.dbf
7    43       USERS                ***     /u01/oradata/dbteste/users01.dbf

List of Temporary Files
=======================
File Size(MB) Tablespace           Maxsize(MB) Tempfile Name
---- -------- -------------------- ----------- --------------------
1    130      TEMP                 32767       /u01/oradata/dbteste/temp01.dbf
```

Identificado os datafiles, utilizamos o comando abaixo para realizar o restore do backup na instância DBTREINA.
```sql
[oracle@lab-ol8-19c => (dbtreina ~]$ rman target /

Recovery Manager: Release 19.0.0.0.0 - Production on Sat Jan 25 21:41:37 2020
Version 19.3.0.0.0

Copyright (c) 1982, 2019, Oracle and/or its affiliates.  All rights reserved.

connected to target database: DBTESTE (DBID=938280311, not open)

RMAN> run {
set newname for datafile 1 to '/u02/oradata/dbtreina/system01.dbf';
set newname for datafile 2 to '/u02/oradata/dbtreina/gosalesreco_data01.dbf';
set newname for datafile 3 to '/u02/oradata/dbtreina/sysaux01.dbf';
set newname for datafile 4 to '/u02/oradata/dbtreina/undotbs01.dbf';
set newname for datafile 5 to '/u02/oradata/dbtreina/gosales_data01.dbf';
set newname for datafile 7 to '/u02/oradata/dbtreina/users01.dbf';
set newname for tempfile 1 to '/u02/oradata/dbtreina/temp01.dbf';
restore database;
}

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

Starting restore at 25/01/2020 22:22:32
using target database control file instead of recovery catalog
allocated channel: ORA_DISK_1
channel ORA_DISK_1: SID=35 device type=DISK

channel ORA_DISK_1: starting datafile backup set restore
channel ORA_DISK_1: specifying datafile(s) to restore from backup set
channel ORA_DISK_1: restoring datafile 00001 to /u02/oradata/dbtreina/system01.dbf
channel ORA_DISK_1: restoring datafile 00002 to /u02/oradata/dbtreina/gosalesreco_data01.dbf
channel ORA_DISK_1: restoring datafile 00003 to /u02/oradata/dbtreina/sysaux01.dbf
channel ORA_DISK_1: restoring datafile 00004 to /u02/oradata/dbtreina/undotbs01.dbf
channel ORA_DISK_1: restoring datafile 00005 to /u02/oradata/dbtreina/gosales_data01.dbf
channel ORA_DISK_1: restoring datafile 00007 to /u02/oradata/dbtreina/users01.dbf
channel ORA_DISK_1: reading from backup piece /u02/backup/dbteste/fisico/files/df_DBTESTE_6_1_1030635610.dbf
channel ORA_DISK_1: piece handle=/u02/backup/dbteste/fisico/files/df_DBTESTE_6_1_1030635610.dbf tag=BKPDBFULL
channel ORA_DISK_1: restored backup piece 1
channel ORA_DISK_1: restore complete, elapsed time: 00:15:25
Finished restore at 25/01/2020 22:38:04

RMAN> switch database to copy;  --comando responsável por atualizar o controlfile com os novos destinos dos datafiles.

datafile 1 switched to datafile copy "/u02/oradata/dbtreina/system01.dbf"
datafile 2 switched to datafile copy "/u02/oradata/dbtreina/gosalesreco_data01.dbf"
datafile 3 switched to datafile copy "/u02/oradata/dbtreina/sysaux01.dbf"
datafile 4 switched to datafile copy "/u02/oradata/dbtreina/undotbs01.dbf"
datafile 5 switched to datafile copy "/u02/oradata/dbtreina/gosales_data01.dbf"
datafile 7 switched to datafile copy "/u02/oradata/dbtreina/users01.dbf"

RMAN> recover database; 

Starting recover at 25/01/2020 22:45:26
using target database control file instead of recovery catalog
allocated channel: ORA_DISK_1
channel ORA_DISK_1: SID=34 device type=DISK

starting media recovery
channel ORA_DISK_1: starting archived log restore to default destination
channel ORA_DISK_1: restoring archived log
archived log thread=1 sequence=31
channel ORA_DISK_1: reading from backup piece /u02/backup/dbteste/fisico/files/arch_DBTESTE_8_1_1030635866.arc
channel ORA_DISK_1: piece handle=/u02/backup/dbteste/fisico/files/arch_DBTESTE_8_1_1030635866.arc tag=BKPARCHIVE
channel ORA_DISK_1: restored backup piece 1
channel ORA_DISK_1: restore complete, elapsed time: 00:01:46
archived log file name=/u01/app/oracle/product/19.3.0.0/dbhome_1/dbs/arch1_31_1027593916.dbf thread=1 sequence=31
archived log file name=/u01/oradata/dbteste/redo01a.log thread=1 sequence=32
archived log file name=/u01/oradata/dbteste/redo02a.log thread=1 sequence=33
archived log file name=/u01/oradata/dbteste/redo04a.log thread=1 sequence=34
archived log file name=/u01/oradata/dbteste/redo05a.log thread=1 sequence=35
archived log file name=/u01/oradata/dbteste/redo03a.log thread=1 sequence=36
media recovery complete, elapsed time: 00:01:31
Finished recover at 25/01/2020 22:48:48

RMAN> exit

Recovery Manager complete.
```

Visto que o restore finalizou com sucesso, é necessário recriar o controlfile na base DBTREINA.
Com o comando abaixo, criamos o backup do controlfile e localizamos seu destino no filesystem.

```sql
SQL>  alter database backup controlfile to trace;

Database altered.

SQL> col tracefile form a100;
SELECT tracefile
  FROM V$SESSION S, V$PROCESS P, V$PARAMETER PA
 WHERE S.PADDR = P.ADDR
   AND UPPER(PA.NAME) = 'USER_DUMP_DEST'
   AND S.SID = sys_context('USERENV','SID');

TRACEFILE
-------------------------------------------------------------------------
/u01/app/oracle/diag/rdbms/dbtreina/dbtreina/trace/dbtreina_ora_3696.trc

SQL> exit
Disconnected from Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
Version 19.3.0.0.0
```

Com a localização do arquivo de trace criado no comando do backup controlfile, extrai-se o conteúdo do arquivo de controle para a criação com o novo nome DBTREINA do banco. O comando seguinte extrai o comando necessário para criação do arquivo de controle e salva o conteúdo no arquivo new_controlfile_dbtreina.sql.
```sql
sed -n '/CREATE.* RESETLOGS/,$p' /u01/app/oracle/diag/rdbms/dbtreina/dbtreina/trace/dbtreina_ora_3696.trc | \
sed '/.*;/q' | \
sed 's/DBTESTE/DBTREINA/g' | \
sed 's/dbteste/dbtreina/g' | \
sed 's/REUSE/SET/g' | \
sed 's/^ARCHIVELOG/NOARCHIVELOG/g'  > new_controlfile_dbtreina.sql
```

Verificando o arquivo ```new_controlfile_dbtreina.sql```, temos o seguinte conteúdo.

```bash
[oracle@lab-ol8-19c => (dbtreina ~]$ cat new_controlfile_dbtreina.sql

CREATE CONTROLFILE SET DATABASE "DBTREINA" RESETLOGS NOARCHIVELOG
    MAXLOGFILES 16
    MAXLOGMEMBERS 3
    MAXDATAFILES 100
    MAXINSTANCES 8
    MAXLOGHISTORY 292
LOGFILE
  GROUP 1 (
    '/u02/oradata/dbtreina/redo01b.log',
    '/u01/oradata/dbtreina/redo01a.log'
  ) SIZE 150M BLOCKSIZE 512,
  GROUP 2 (
    '/u02/oradata/dbtreina/redo02b.log',
    '/u01/oradata/dbtreina/redo02a.log'
  ) SIZE 150M BLOCKSIZE 512,
  GROUP 3 (
    '/u02/oradata/dbtreina/redo03b.log',
    '/u01/oradata/dbtreina/redo03a.log'
  ) SIZE 150M BLOCKSIZE 512,
  GROUP 4 (
    '/u01/oradata/dbtreina/redo04a.log',
    '/u02/oradata/dbtreina/redo04b.log'
  ) SIZE 150M BLOCKSIZE 512,
  GROUP 5 (
    '/u01/oradata/dbtreina/redo05a.log',
    '/u02/oradata/dbtreina/redo05b.log'
  ) SIZE 150M BLOCKSIZE 512
-- STANDBY LOGFILE
DATAFILE
  '/u02/oradata/dbtreina/system01.dbf',
  '/u02/oradata/dbtreina/gosalesreco_data01.dbf',
  '/u02/oradata/dbtreina/sysaux01.dbf',
  '/u02/oradata/dbtreina/undotbs01.dbf',
  '/u02/oradata/dbtreina/gosales_data01.dbf',
  '/u02/oradata/dbtreina/users01.dbf'
CHARACTER SET AL32UTF8;
```

Iniciando a instância em nomunt e criando o controlfile. 
Primeiro, é necessário alterar o nome do parâmetro ```db_name``` para dbtreina.
```sql
[oracle@lab-ol8-19c => (dbtreina ~]$ sqlplus / as sysdba

SQL*Plus: Release 19.0.0.0.0 - Production on Sat Jan 25 23:13:59 2020
Version 19.3.0.0.0

Copyright (c) 1982, 2019, Oracle.  All rights reserved.

Connected to:
Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
Version 19.3.0.0.0

SQL> alter system set db_name=dbtreina scope = spfile;

System altered.

SQL> startup nomount force
ORACLE instance started.

Total System Global Area  243268216 bytes
Fixed Size                  8895096 bytes
Variable Size             180355072 bytes
Database Buffers           50331648 bytes
Redo Buffers                3686400 bytes
SQL>

SQL> @new_controlfile_dbtreina.sql

Control file created.
```
Realizado a criação do controlfile, agora podemos abrir o banco e adicionar uma tablespace TEMP.
```sql
SQL> alter database open resetlogs;

Database altered.

SQL> alter tablespace temp add tempfile '/u02/oradata/dbtreina/temp01.dbf' size 3G;

Tablespace altered.
```

Checando o status do banco de dados.
```sql
select database_role, NAME, status, OPEN_MODE, LOG_MODE, GUARD_STATUS, TO_CHAR(Current_scn, '9999999999999999') "Current SCN", resetlogs_time from v$database, v$instance;

DATABASE_ROLE    NAME      STATUS       OPEN_MODE      LOG_MODE     GUARD_S Current SCN   RESETLOGS_TIME
---------------- --------- ------------ -------------- ------------ ------- -----------   ----------------
PRIMARY          DBTREINA  OPEN         READ WRITE      NOARCHIVELOG NONE    3556996       25/01/2020 23:26:25

```

Pronto, backup restaurado em uma nova instância com sucesso 👏👏👏👏 :). Não esqueça de criar o arquivo de senha, nesse caso pode-se copiar o arquivo do banco de origem do backup.

**Muito bom!!!** neste artigo realizamos a importação de uma tabela na base de teste, remapeando para um novo nome como tabela de backup. Realizamos a restauração de backup em uma nova instância, criando a mesma com os parâmetros necessário para o restore, resolvemos os últimos itens **1.8**, e **2.1** que estava faltando.  

E isso é tudo, espero que essa série de artigos ajude você no dia a dia. hahahaha

Até o próximo artigo PARTE 4 e vamos em frente!!!

#FocoForçaFé

[Michel Souza](https://www.linkedin.com/in/michel-ferreira-souza/)

 