
## Log do active duplicate database utilizando nofilenamecheck no mesmo ambiente target.

Esse log apresenta o procedimento de uma clonagem de base com a instância dbprod e dbteste no mesmo servidor e usando a opção **nofilenamecheck**, sem informar os parâmetros db_file_name_convert e log_file_name_convert no SPFILE.

Esse procedimento corrompeu e sobrescreveu os arquivos do ambiente target.

Log Rman:
```sql
[oracle@lab ~]$  rman target sys/891866@dbprod auxiliary sys/891866@dbteste

Recovery Manager: Release 11.2.0.4.0 - Production on Sun Feb 2 00:29:21 2020

Copyright (c) 1982, 2011, Oracle and/or its affiliates.  All rights reserved.

connected to target database: DBPROD (DBID=1130215834)
connected to auxiliary database: DBTESTE (not mounted)

RMAN>

RMAN>

RMAN> run {
allocate channel aux1 type disk;
allocate auxiliary channel aux2 type disk;
SQL 'alter system switch logfile';
duplicate target database to dbteste from active database nofilenamecheck;
}2> 3> 4> 5> 6>

using target database control file instead of recovery catalog
allocated channel: aux1
channel aux1: SID=126 device type=DISK

allocated channel: aux2
channel aux2: SID=355 device type=DISK

sql statement: alter system switch logfile

Starting Duplicate Db at 02-FEB-20

contents of Memory Script:
{
   sql clone "alter system set  control_files =
  ''+DGDATA/dbteste/controlfile/current.262.1031149911'', ''+DGDATA/dbteste/controlfile/current.263.1031149911'' comment=
 ''Set by RMAN'' scope=spfile";
   sql clone "alter system set  db_name =
 ''DBPROD'' comment=
 ''Modified by RMAN duplicate'' scope=spfile";
   sql clone "alter system set  db_unique_name =
 ''DBTESTE'' comment=
 ''Modified by RMAN duplicate'' scope=spfile";
   shutdown clone immediate;
   startup clone force nomount
   backup as copy current controlfile auxiliary format  '+DGDATA/dbteste/controlfile/current.262.1031149911';
   restore clone controlfile to  '+DGDATA/dbteste/controlfile/current.263.1031149911' from
 '+DGDATA/dbteste/controlfile/current.262.1031149911';
   sql clone "alter system set  control_files =
  ''+DGDATA/dbteste/controlfile/current.262.1031149911'', ''+DGDATA/dbteste/controlfile/current.263.1031149911'' comment=
 ''Set by RMAN'' scope=spfile";
   shutdown clone immediate;
   startup clone nomount;
   alter clone database mount;
}
executing Memory Script

sql statement: alter system set  control_files =   ''+DGDATA/dbteste/controlfile/current.262.1031149911'', ''+DGDATA/dbteste/controlfile/current.263.1031149911'' comment= ''Set by RMAN'' scope=spfile

sql statement: alter system set  db_name =  ''DBPROD'' comment= ''Modified by RMAN duplicate'' scope=spfile

sql statement: alter system set  db_unique_name =  ''DBTESTE'' comment= ''Modified by RMAN duplicate'' scope=spfile

Oracle instance shut down

Oracle instance started

Total System Global Area    1603411968 bytes

Fixed Size                     2253664 bytes
Variable Size                520096928 bytes
Database Buffers            1073741824 bytes
Redo Buffers                   7319552 bytes
allocated channel: aux2
channel aux2: SID=123 device type=DISK

Starting backup at 02-FEB-20
channel aux1: starting datafile copy
copying current control file
output file name=/orabin01/app/oracle/product/11.2.0.4/dbhome_1/dbs/snapcf_dbprod.f tag=TAG20200202T003022 RECID=23 STAMP=1031272222
channel aux1: datafile copy complete, elapsed time: 00:00:03
Finished backup at 02-FEB-20

Starting restore at 02-FEB-20

channel aux2: copied control file copy
Finished restore at 02-FEB-20

sql statement: alter system set  control_files =   ''+DGDATA/dbteste/controlfile/current.262.1031149911'', ''+DGDATA/dbteste/controlfile/current.263.1031149911'' comment= ''Set by RMAN'' scope=spfile

Oracle instance shut down

connected to auxiliary database (not started)
Oracle instance started

Total System Global Area    1603411968 bytes

Fixed Size                     2253664 bytes
Variable Size                520096928 bytes
Database Buffers            1073741824 bytes
Redo Buffers                   7319552 bytes
allocated channel: aux2
channel aux2: SID=123 device type=DISK

database mounted
RMAN-05529: WARNING: DB_FILE_NAME_CONVERT resulted in invalid ASM names; names changed to disk group only.
Using previous duplicated file +DGDATA/dbprod/datafile/data_teste01.dbf for datafile 6 with checkpoint SCN of 1170653

contents of Memory Script:
{
   set newname for datafile  1 to
 "+dgdata";
   set newname for datafile  2 to
 "+dgdata";
   set newname for datafile  3 to
 "+dgdata";
   set newname for datafile  4 to
 "+dgdata";
   set newname for datafile  5 to
 "+dgdata";
   set newname for datafile  6 to
 "+DGDATA/dbprod/datafile/data_teste01.dbf";
   backup as copy reuse
   datafile  1 auxiliary format
 "+dgdata"   datafile
 2 auxiliary format
 "+dgdata"   datafile
 3 auxiliary format
 "+dgdata"   datafile
 4 auxiliary format
 "+dgdata"   datafile
 5 auxiliary format
 "+dgdata"   ;
   sql 'alter system archive log current';
}
executing Memory Script

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

Starting backup at 02-FEB-20
channel aux1: starting datafile copy
input datafile file number=00001 name=+DGDATA/dbprod/datafile/system.256.1031136205
output file name=+DGDATA/dbteste/datafile/system.315.1031272245 tag=TAG20200202T003044
channel aux1: datafile copy complete, elapsed time: 00:00:35
channel aux1: starting datafile copy
input datafile file number=00002 name=+DGDATA/dbprod/datafile/sysaux.257.1031136207
output file name=+DGDATA/dbteste/datafile/sysaux.310.1031272281 tag=TAG20200202T003044
channel aux1: datafile copy complete, elapsed time: 00:00:15
channel aux1: starting datafile copy
input datafile file number=00003 name=+DGDATA/dbprod/datafile/undotbs1.258.1031136207
output file name=+DGDATA/dbteste/datafile/undotbs1.307.1031272295 tag=TAG20200202T003044
channel aux1: datafile copy complete, elapsed time: 00:00:15
channel aux1: starting datafile copy
input datafile file number=00005 name=+DGDATA/dbprod/datafile/example.269.1031136307
output file name=+DGDATA/dbteste/datafile/example.296.1031272311 tag=TAG20200202T003044
channel aux1: datafile copy complete, elapsed time: 00:00:15
channel aux1: starting datafile copy
input datafile file number=00004 name=+DGDATA/dbprod/datafile/users.259.1031136207
output file name=+DGDATA/dbteste/datafile/users.295.1031272325 tag=TAG20200202T003044
channel aux1: datafile copy complete, elapsed time: 00:00:01
Finished backup at 02-FEB-20

sql statement: alter system archive log current

contents of Memory Script:
{
   backup as copy reuse
   archivelog like  "/orabin01/archive/dbprod_1_153_1031136284.arc" auxiliary format
 "+DGDATA"   archivelog like
 "/orabin01/archive/dbprod_1_154_1031136284.arc" auxiliary format
 "+DGDATA"   archivelog like
 "/orabin01/archive/dbprod_1_155_1031136284.arc" auxiliary format
 "+DGDATA"   ;
   catalog clone start with  "+DGDATA";
   catalog clone datafilecopy  "+DGDATA/dbprod/datafile/data_teste01.dbf";
   switch clone datafile  6 to datafilecopy
 "+DGDATA/dbprod/datafile/data_teste01.dbf";
   switch clone datafile all;
}
executing Memory Script

Starting backup at 02-FEB-20
channel aux1: starting archived log copy
input archived log thread=1 sequence=153 RECID=43 STAMP=1031272095
output file name=+DGDATA/dbteste/archivelog/2020_02_02/thread_1_seq_153.309.1031272327 RECID=0 STAMP=0
channel aux1: archived log copy complete, elapsed time: 00:00:01
channel aux1: starting archived log copy
input archived log thread=1 sequence=154 RECID=44 STAMP=1031272210
output file name=+DGDATA/dbteste/archivelog/2020_02_02/thread_1_seq_154.303.1031272329 RECID=0 STAMP=0
channel aux1: archived log copy complete, elapsed time: 00:00:01
channel aux1: starting archived log copy
input archived log thread=1 sequence=155 RECID=45 STAMP=1031272327
output file name=+DGDATA/dbteste/archivelog/2020_02_02/thread_1_seq_155.306.1031272329 RECID=0 STAMP=0
channel aux1: archived log copy complete, elapsed time: 00:00:01
Finished backup at 02-FEB-20

searching for all files that match the pattern +DGDATA

List of Files Unknown to the Database
=====================================
File Name: +dgdata/DBTESTE/spfiledbteste.ora
File Name: +dgdata/DBTESTE/ARCHIVELOG/2020_02_02/thread_1_seq_153.309.1031272327
File Name: +dgdata/DBTESTE/ARCHIVELOG/2020_02_02/thread_1_seq_154.303.1031272329
File Name: +dgdata/DBTESTE/ARCHIVELOG/2020_02_02/thread_1_seq_155.306.1031272329
File Name: +dgdata/DBTESTE/TEMPFILE/TEMP.277.1031149929
File Name: +dgdata/DBTESTE/TEMPFILE/TEMP.304.1031266553
File Name: +dgdata/DBTESTE/TEMPFILE/TEMP.314.1031270327
File Name: +dgdata/DBTESTE/ONLINELOG/group_1.271.1031149913
File Name: +dgdata/DBTESTE/ONLINELOG/group_1.272.1031149915
File Name: +dgdata/DBTESTE/ONLINELOG/group_2.273.1031149915
File Name: +dgdata/DBTESTE/ONLINELOG/group_2.274.1031149917
File Name: +dgdata/DBTESTE/ONLINELOG/group_3.275.1031149917
File Name: +dgdata/DBTESTE/ONLINELOG/group_3.276.1031149919
File Name: +dgdata/DBTESTE/ONLINELOG/group_1.311.1031270321
File Name: +dgdata/DBTESTE/ONLINELOG/group_2.312.1031270323
File Name: +dgdata/DBTESTE/ONLINELOG/group_3.313.1031270323
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.266.1031253551
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.267.1031253551
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.265.1031256781
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.264.1031256781
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.283.1031391455
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.284.1031257161
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.285.1031257605
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.286.1031257605
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.287.1031258873
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.288.1031258873
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.289.1031259107
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.290.1031259107
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.291.1031259369
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.278.1031259587
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.281.1031694771
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.294.1031260123
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.293.1031260123
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.297.1031260483
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.298.1031260483
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.299.1031266411
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.300.1031266411
File Name: +dgdata/DBTESTE/DATAFILE/SYSTEM.315.1031272245
File Name: +dgdata/DBTESTE/DATAFILE/SYSAUX.310.1031272281
File Name: +dgdata/DBTESTE/DATAFILE/UNDOTBS1.307.1031272295
File Name: +dgdata/DBTESTE/DATAFILE/EXAMPLE.296.1031272311
File Name: +dgdata/DBTESTE/DATAFILE/USERS.295.1031272325
File Name: +dgdata/DBPROD/spfiledbprod.ora
File Name: +dgdata/DBPROD/CONTROLFILE/Current.261.1031136283
File Name: +dgdata/DBPROD/CONTROLFILE/Current.260.1031136283
cataloging files...
cataloging done

List of Cataloged Files
=======================
File Name: +dgdata/DBTESTE/ARCHIVELOG/2020_02_02/thread_1_seq_153.309.1031272327
File Name: +dgdata/DBTESTE/ARCHIVELOG/2020_02_02/thread_1_seq_154.303.1031272329
File Name: +dgdata/DBTESTE/ARCHIVELOG/2020_02_02/thread_1_seq_155.306.1031272329
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.267.1031253551
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.264.1031256781
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.284.1031257161
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.286.1031257605
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.288.1031258873
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.290.1031259107
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.281.1031694771
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.293.1031260123
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.298.1031260483
File Name: +dgdata/DBTESTE/DATAFILE/SYSTEM.315.1031272245
File Name: +dgdata/DBTESTE/DATAFILE/SYSAUX.310.1031272281
File Name: +dgdata/DBTESTE/DATAFILE/UNDOTBS1.307.1031272295
File Name: +dgdata/DBTESTE/DATAFILE/EXAMPLE.296.1031272311
File Name: +dgdata/DBTESTE/DATAFILE/USERS.295.1031272325

List of Files Which Where Not Cataloged
=======================================
File Name: +dgdata/DBTESTE/spfiledbteste.ora
  RMAN-07518: Reason: Foreign database file DBID: 0  Database Name:
File Name: +dgdata/DBTESTE/TEMPFILE/TEMP.277.1031149929
  RMAN-07518: Reason: Foreign database file DBID: 941836310  Database Name: DBTESTE
File Name: +dgdata/DBTESTE/TEMPFILE/TEMP.304.1031266553
  RMAN-07518: Reason: Foreign database file DBID: 941952942  Database Name: DBTESTE
File Name: +dgdata/DBTESTE/TEMPFILE/TEMP.314.1031270327
  RMAN-07518: Reason: Foreign database file DBID: 941956720  Database Name: DBTESTE
File Name: +dgdata/DBTESTE/ONLINELOG/group_1.271.1031149913
  RMAN-07518: Reason: Foreign database file DBID: 941836310  Database Name: DBTESTE
File Name: +dgdata/DBTESTE/ONLINELOG/group_1.272.1031149915
  RMAN-07518: Reason: Foreign database file DBID: 941836310  Database Name: DBTESTE
File Name: +dgdata/DBTESTE/ONLINELOG/group_2.273.1031149915
  RMAN-07518: Reason: Foreign database file DBID: 941836310  Database Name: DBTESTE
File Name: +dgdata/DBTESTE/ONLINELOG/group_2.274.1031149917
  RMAN-07518: Reason: Foreign database file DBID: 941836310  Database Name: DBTESTE
File Name: +dgdata/DBTESTE/ONLINELOG/group_3.275.1031149917
  RMAN-07518: Reason: Foreign database file DBID: 941836310  Database Name: DBTESTE
File Name: +dgdata/DBTESTE/ONLINELOG/group_3.276.1031149919
  RMAN-07518: Reason: Foreign database file DBID: 941836310  Database Name: DBTESTE
File Name: +dgdata/DBTESTE/ONLINELOG/group_1.311.1031270321
  RMAN-07518: Reason: Foreign database file DBID: 941956720  Database Name: DBTESTE
File Name: +dgdata/DBTESTE/ONLINELOG/group_2.312.1031270323
  RMAN-07518: Reason: Foreign database file DBID: 941956720  Database Name: DBTESTE
File Name: +dgdata/DBTESTE/ONLINELOG/group_3.313.1031270323
  RMAN-07518: Reason: Foreign database file DBID: 941956720  Database Name: DBTESTE
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.266.1031253551
  RMAN-07517: Reason: The file header is corrupted
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.265.1031256781
  RMAN-07517: Reason: The file header is corrupted
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.283.1031391455
  RMAN-07517: Reason: The file header is corrupted
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.285.1031257605
  RMAN-07517: Reason: The file header is corrupted
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.287.1031258873
  RMAN-07517: Reason: The file header is corrupted
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.289.1031259107
  RMAN-07517: Reason: The file header is corrupted
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.291.1031259369
  RMAN-07517: Reason: The file header is corrupted
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.278.1031259587
  RMAN-07517: Reason: The file header is corrupted
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.294.1031260123
  RMAN-07517: Reason: The file header is corrupted
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.297.1031260483
  RMAN-07517: Reason: The file header is corrupted
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.299.1031266411
  RMAN-07517: Reason: The file header is corrupted
File Name: +dgdata/DBTESTE/CONTROLFILE/Current.300.1031266411
  RMAN-07518: Reason: Foreign database file DBID: 941956720  Database Name: DBTESTE
File Name: +dgdata/DBPROD/spfiledbprod.ora
  RMAN-07518: Reason: Foreign database file DBID: 0  Database Name:
File Name: +dgdata/DBPROD/CONTROLFILE/Current.261.1031136283
  RMAN-07519: Reason: Error while cataloging. See alert.log.
File Name: +dgdata/DBPROD/CONTROLFILE/Current.260.1031136283
  RMAN-07519: Reason: Error while cataloging. See alert.log.

cataloged datafile copy
datafile copy file name=+DGDATA/dbprod/datafile/data_teste01.dbf RECID=37 STAMP=1031272332

datafile 6 switched to datafile copy
input datafile copy RECID=37 STAMP=1031272332 file name=+DGDATA/dbprod/datafile/data_teste01.dbf

datafile 1 switched to datafile copy
input datafile copy RECID=38 STAMP=1031272333 file name=+DGDATA/dbteste/datafile/system.315.1031272245
datafile 2 switched to datafile copy
input datafile copy RECID=39 STAMP=1031272333 file name=+DGDATA/dbteste/datafile/sysaux.310.1031272281
datafile 3 switched to datafile copy
input datafile copy RECID=40 STAMP=1031272333 file name=+DGDATA/dbteste/datafile/undotbs1.307.1031272295
datafile 4 switched to datafile copy
input datafile copy RECID=41 STAMP=1031272333 file name=+DGDATA/dbteste/datafile/users.295.1031272325
datafile 5 switched to datafile copy
input datafile copy RECID=42 STAMP=1031272333 file name=+DGDATA/dbteste/datafile/example.296.1031272311

contents of Memory Script:
{
   set until scn  1171103;
   recover
   clone database
    delete archivelog
   ;
}
executing Memory Script

executing command: SET until clause

Starting recover at 02-FEB-20

starting media recovery

archived log for thread 1 with sequence 155 is already on disk as file +DGDATA/dbteste/archivelog/2020_02_02/thread_1_seq_155.306.1031272329
archived log file name=+DGDATA/dbteste/archivelog/2020_02_02/thread_1_seq_155.306.1031272329 thread=1 sequence=155
Oracle Error:
ORA-01547: warning: RECOVER succeeded but OPEN RESETLOGS would get error below
ORA-01194: file 6 needs more recovery to be consistent
ORA-01110: data file 6: '+DGDATA/dbprod/datafile/data_teste01.dbf'

media recovery complete, elapsed time: 00:00:00
Finished recover at 02-FEB-20
Oracle instance started

Total System Global Area    1603411968 bytes

Fixed Size                     2253664 bytes
Variable Size                520096928 bytes
Database Buffers            1073741824 bytes
Redo Buffers                   7319552 bytes

contents of Memory Script:
{
   sql clone "alter system set  db_name =
 ''DBTESTE'' comment=
 ''Reset to original value by RMAN'' scope=spfile";
   sql clone "alter system reset  db_unique_name scope=spfile";
   shutdown clone immediate;
   startup clone nomount;
}
executing Memory Script

sql statement: alter system set  db_name =  ''DBTESTE'' comment= ''Reset to original value by RMAN'' scope=spfile

sql statement: alter system reset  db_unique_name scope=spfile

Oracle instance shut down

connected to auxiliary database (not started)
Oracle instance started

Total System Global Area    1603411968 bytes

Fixed Size                     2253664 bytes
Variable Size                520096928 bytes
Database Buffers            1073741824 bytes
Redo Buffers                   7319552 bytes
allocated channel: aux2
channel aux2: SID=123 device type=DISK
sql statement: CREATE CONTROLFILE REUSE SET DATABASE "DBTESTE" RESETLOGS ARCHIVELOG
  MAXLOGFILES     16
  MAXLOGMEMBERS      3
  MAXDATAFILES      100
  MAXINSTANCES     8
  MAXLOGHISTORY      292
 LOGFILE
  GROUP   1  SIZE 50 M ,
  GROUP   2  SIZE 50 M ,
  GROUP   3  SIZE 50 M
 DATAFILE
  '+DGDATA/dbteste/datafile/system.315.1031272245'
 CHARACTER SET AL32UTF8


contents of Memory Script:
{
   set newname for tempfile  1 to
 "+dgdata";
   switch clone tempfile all;
   catalog clone datafilecopy  "+DGDATA/dbteste/datafile/sysaux.310.1031272281",
 "+DGDATA/dbteste/datafile/undotbs1.307.1031272295",
 "+DGDATA/dbteste/datafile/users.295.1031272325",
 "+DGDATA/dbteste/datafile/example.296.1031272311",
 "+DGDATA/dbprod/datafile/data_teste01.dbf";
   switch clone datafile all;
}
executing Memory Script

executing command: SET NEWNAME

renamed tempfile 1 to +dgdata in control file

cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/sysaux.310.1031272281 RECID=1 STAMP=1031272353
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/undotbs1.307.1031272295 RECID=2 STAMP=1031272353
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/users.295.1031272325 RECID=3 STAMP=1031272353
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/example.296.1031272311 RECID=4 STAMP=1031272353
cataloged datafile copy
datafile copy file name=+DGDATA/dbprod/datafile/data_teste01.dbf RECID=5 STAMP=1031272353

datafile 2 switched to datafile copy
input datafile copy RECID=1 STAMP=1031272353 file name=+DGDATA/dbteste/datafile/sysaux.310.1031272281
datafile 3 switched to datafile copy
input datafile copy RECID=2 STAMP=1031272353 file name=+DGDATA/dbteste/datafile/undotbs1.307.1031272295
datafile 4 switched to datafile copy
input datafile copy RECID=3 STAMP=1031272353 file name=+DGDATA/dbteste/datafile/users.295.1031272325
datafile 5 switched to datafile copy
input datafile copy RECID=4 STAMP=1031272353 file name=+DGDATA/dbteste/datafile/example.296.1031272311
datafile 6 switched to datafile copy
input datafile copy RECID=6 STAMP=1031272353 file name=+DGDATA/dbprod/datafile/data_teste01.dbf

contents of Memory Script:
{
   Alter clone database open resetlogs;
}
executing Memory Script

released channel: aux1
released channel: aux2
RMAN-00571: ===========================================================
RMAN-00569: =============== ERROR MESSAGE STACK FOLLOWS ===============
RMAN-00571: ===========================================================
RMAN-03002: failure of Duplicate Db command at 02/02/2020 00:32:33
RMAN-05501: aborting duplication of target database
RMAN-03015: error occurred in stored script Memory Script
RMAN-06136: ORACLE error from auxiliary database: ORA-01194: file 6 needs more recovery to be consistent
ORA-01110: data file 6: '+DGDATA/dbprod/datafile/data_teste01.dbf'

RMAN>


```