

<img src="../img/teste_dba_4.png" alt="" width="80%">

# Resolvendo teste para vaga de (DBA) - PARTE 5
##### Publicado em 12/01/2020 por [Michel Souza](https://www.linkedin.com/in/michel-ferreira-souza/)

Fala galera, continuando com a série aprendendo com **teste de DBA para entrevista de emprego**. Neste post darei continuidade da [Parte 4](https://github.com/souzacristsf/article_database/blob/master/ORACLE/TESTE_DBA/test_solved_parte-3.md), apresentando a resolução de um teste para vaga de emprego de DBA. <br> 

> *"A melhor forma de aprender é ensinando ou compartilhando conhecimento."* 

# Teste Prático DBA Oracle
Para realizar o teste o recrutador informa o acesso que normalmente é via SSH. Abaixo segue o ambiente utilizado para realizar o teste.

```
Sistema Operacional : Oracle Linux 7.6 64 Bits
Database Version    : Oracle Enterprise 19C
```

  1. Crie um Non-CDB database com nome de DBTESTE on Group ASM "+DATA" ou filesystem local.
        <ol>
          <ul>1.1 Crie Non-CDB com os valores abaixo. (Resolvido <a href="https://github.com/souzacristsf/article_database/blob/master/ORACLE/TESTE_DBA/test_solved.md">Parte - 1</a>).
            <table>
              <tr>
                  <th>Requisito</th>
                  <th>Valor</th>
              </tr>
              <tr>
                  <td>CHARACTERSET</td>
                  <td>AL32UTF8</td>
              </tr>
              <tr>
                  <td>LANGUAGE</td>
                  <td>AMERICAN</td>
              </tr><tr>
                  <td>TERRITORY</td>
                  <td>AMERICA</td>
              </tr><tr>
                  <td>PASSWORD SYS</td>
                  <td>Manager19cTST</td>
              </tr>
              <tr>
                  <td>PASSWORD SYSTEM</td>
                  <td>Manager19cTST</td>
              </tr>
              <tr>
                  <td>MEMORY</td>
                  <td>ASMM</td>
              </tr>
            </table>
          </ul> <br>
          <ul>1.2 Multiplexar os Redolog criando 3 x 100MB e controlfile on Diskgroup "+DATA" ou filesystem local. (Resolvido <a href="https://github.com/souzacristsf/article_database/blob/master/ORACLE/TESTE_DBA/test_solved_parte-2.md">Parte - 2</a>).
          </ul> <br>
          <ul>1.3 Crie uma Tablespace BIGFILE GOSALES_TS com 10 GB autoextend com 256M e extent management local autoallocate na instância DBTESTE. (Resolvido <a href="https://github.com/souzacristsf/article_database/blob/master/ORACLE/TESTE_DBA/test_solved_parte-3.md">Parte - 3</a>). </ul><br> 
          <ul>1.4 Gerar um Dump FULL do cdbprd da instância dbprod e armazenar o dump no diretorio em '/home/oracle/backup/logico/'. (Resolvido <a href="https://github.com/souzacristsf/article_database/blob/master/ORACLE/TESTE_DBA/test_solved_parte-3.md">Parte - 3</a>).</ul><br>
          <>1.5 Importe o Schema GOSALESDW e aplique todos os grants existentes na base origem. (Resolvido <a href="https://github.com/souzacristsf/article_database/blob/master/ORACLE/TESTE_DBA/test_solved_parte-4.md">Parte - 4</a>).</ul><br>
          <ul>1.6 Colocar o banco em Modo Archivelog. (Resolvido <a href="https://github.com/souzacristsf/article_database/blob/master/ORACLE/TESTE_DBA/test_solved_parte-4.md">Parte - 4</a>).</ul><br>
          <ul>1.7 Execute um Backup RMAN Full da instância dbprod. (Resolvido <a href="https://github.com/souzacristsf/article_database/blob/master/ORACLE/TESTE_DBA/test_solved_parte-4.md">Parte - 4</a>). </ul><br>
          <ul>1.8 Importar a tabela EMP_RANKING_DIM com nome EMP_RANKING_DIM_BKP, no schema GOSALESDW em uma nova tablespace chamada GOSALES_TSRECO.<ul>
        </ol>

2. Clone Database <br>
    2.1 Crie uma nova instância DBTREINA a partir do RMAN FULL da base DBTESTE <br>

# Solução 
## Realizando a importação da tabela EMP_RANKING_DIM 
Resposta **1.6**) Para a importação da tabela ```EMP_RANKING_DIM```, irei utilizar o dump gerado na [Parte 3](https://github.com/souzacristsf/article_database/blob/master/ORACLE/TESTE_DBA/test_solved_parte-3.md) do artigo. <br>

Antes de realizar a importação da tabela, vamos conectar no ambiente ```DBPROD``` e coletar algumas informações do objeto que será importado na base ```DBTESTE```.
```sql
alter session set nls_date_format='dd/mm/yyyy hh24:mi:ss';
set line 1000;
set pages 1000;
col object_name form a20;
col TABLESPACE_NAME form a20;
col owner form a10;
select 
  b.TABLESPACE_NAME
, a.owner
, a.object_name
, a.object_type
, a.created
, a.last_ddl_time
, a.TIMESTAMP 
from dba_objects a, dba_tables b 
where TABLE_NAME = object_name 
and object_name like '%EMP_RANKING_DIM%'
and a.owner = 'GOSALESDW';

TABLESPACE_NAME    OWNER      OBJECT_NAME        OBJECT_TYPE    CREATED             LAST_DDL_TIME       TIMESTAMP
------------------ ---------- ------------------ -------------- ------------------- ------------------- -------------------
GOSALES_TS         GOSALESDW  EMP_RANKING_DIM    TABLE           21/12/2019 11:27:03 21/12/2019 11:27:03 2019-12-21:11:27:03
```
> **Observação**: é importante saber a tablespace de origem para o remapeamento para a nova tablespace.

Para realizar a importação da tabela é necessário criar a tablespace ```GOSALES_TSRECO``` e conceder **quota** para o usuário **GOSALESDW** na tablespace **GOSALES_TSRECO**, conforme comando abaixo.
```sql
CREATE TABLESPACE "GOSALES_TSRECO" DATAFILE '/u01/oradata/dbteste/gosalesreco_data01.dbf' size 100m autoextend on next 100M maxsize 512m;

Tablespace created.

alter user GOSALESDW quota unlimited on GOSALES_TSRECO;
```

Para iniciar o processo de importação dos dados, renomeando a tabela EMP_RANKING_DIM para **EMP_RANKING_DIM_BKP** e remapear/importar para uma nova tablespace, utilizamos o seguinte comando.
```sql
impdp teste/teste@DBTESTE DIRECTORY=DATA_PUMP TABLES=GOSALESDW.EMP_RANKING_DIM REMAP_TABLE=GOSALESDW.EMP_RANKING_DIM:EMP_RANKING_DIM_BKP REMAP_TABLESPACE=GOSALES_TS:GOSALES_TSRECO DUMPFILE=DBPROD.FULL%U.2019123103.dmp logfile=impdpDBTESTE.GOSALESDW.EMP_RANKING_DIM_REMAPT.`date +%Y%m%d%H`.log
```

```sql
Import: Release 19.0.0.0.0 - Production on Mon Jan 20 21:41:39 2020
Version 19.3.0.0.0

Copyright (c) 1982, 2019, Oracle and/or its affiliates.  All rights reserved.

Connected to: Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
Master table "TESTE"."SYS_IMPORT_TABLE_01" successfully loaded/unloaded
import done in AL32UTF8 character set and UTF8 NCHAR character set
export done in AL32UTF8 character set and AL16UTF16 NCHAR character set
Warning: possible data loss in character set conversions
Starting "TESTE"."SYS_IMPORT_TABLE_01":  teste/********@DBTESTE DIRECTORY=DATA_PUMP TABLES=GOSALESDW.EMP_RANKING_DIM REMAP_TABLE=GOSALESDW.EMP_RANKING_DIM:EMP_RANKING_DIM_BKP REMAP_TABLESPACE=GOSALES_TS:GOSALES_TSRECO DUMPFILE=DBPROD.FULL%U.2019123103.dmp logfile=impdpDBTESTE.GOSALESDW.EMP_RANKING_DIM_REMAPT.2020012021.log
Processing object type DATABASE_EXPORT/SCHEMA/TABLE/TABLE
Processing object type DATABASE_EXPORT/SCHEMA/TABLE/TABLE_DATA
. . imported "GOSALESDW"."EMP_RANKING_DIM_BKP"           20.88 KB       5 rows
Processing object type DATABASE_EXPORT/SCHEMA/TABLE/STATISTICS/TABLE_STATISTICS
Processing object type DATABASE_EXPORT/STATISTICS/MARKER
Job "TESTE"."SYS_IMPORT_TABLE_01" successfully completed at Mon Jan 20 21:41:57 2020 elapsed 0 00:00:16
```
> **Observação**: visto que nenhum erro ocorreu na importação dos dados. O erro abaixo pode ocorrer quando o nome da constraint já existe. Nesse caso pode-se utilizar o parâmetro **EXCLUDE=CONSTRAINT** e caso necessite carregar apenas os dados em uma ou mais tabelas existentes, utiliza-se o parâmetro **content=DATA_ONLY** ou **TABLE_EXISTS_ACTION**, veja mais detalhes [aqui](https://docs.oracle.com/database/121/SUTIL/GUID-C9664F8C-19C5-4177-AC20-5682AEABA07F.htm#SUTIL936).
```sql
Processing object type DATABASE_EXPORT/SCHEMA/TABLE/TABLE
ORA-39083: Object type TABLE:"<schema>"."<table>" failed to create with error:
ORA-02264: name already used by an existing constraint

Processing object type TABLE_EXPORT/TABLE/CONSTRAINT/CONSTRAINT 
ORA-31684: Object type CONSTRAINT:"GOSALESDW"."C_PRD_AMOUNT" already exists
ORA-31684: Object type CONSTRAINT:"GOSALESDW"."PRODUCT_ID_PK" already exists
```
Com a importação realizada, basta verificar se a tabela de backup existe. 
```sql
alter session set nls_date_format='dd/mm/yyyy hh24:mi:ss';
set line 1000;
set pages 1000;
col object_name form a20;
col TABLESPACE_NAME form a20;
col owner form a10;
select 
  b.TABLESPACE_NAME
, a.owner
, a.object_name
, a.object_type
, a.created
, a.last_ddl_time
, a.TIMESTAMP 
from dba_objects a, dba_tables b 
where TABLE_NAME = object_name 
and object_name = 'EMP_RANKING_DIM_BKP'
and a.owner = 'GOSALESDW';

TABLESPACE_NAME      OWNER      OBJECT_NAME          OBJECT_TYPE             CREATED             LAST_DDL_TIME       TIMESTAMP
-------------------- ---------- -------------------- ----------------------- ------------------- ------------------- -------------------
GOSALES_TSRECO       GOSALESDW  EMP_RANKING_DIM_BKP  TABLE                   20/01/2020 21:41:52 20/01/2020 21:41:52 2020-01-20:21:41:52
```
## Restaurando backup físico da base DBTESTE em uma nova instância DBTREINA no mesmo servidor <br>
Resposta **2.1**) Resolvi essa questão em um artigo separado, acompanhe [aqui](https://github.com/souzacristsf/article_database/blob/master/ORACLE/RESTORE/restore_backup_new_instance.md).<br>

**Muito bom!!!** neste artigo realizamos a importação de uma tabela na base de teste, remapeando com um novo nome. Realizamos a restauração de backup em uma nova instância, criando a mesma com os parâmetros necessário para o restore, resolvemos os últimos itens **1.8**, e **2.1** que estava faltando.  

E isso é tudo, espero que essa série de artigos ajude você no dia a dia. hahahaha

Até o próximo artigo e vamos em frente!!!

#FocoForçaFé

[Michel Souza](https://www.linkedin.com/in/michel-ferreira-souza/)

 