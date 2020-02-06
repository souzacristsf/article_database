## Log do Active Duplicate Database com erro ORA-19625 no final do processo de clonagem de base.

```sql
[oracle@lab ~]$  rman target sys/891866@dbprod auxiliary sys/891866@dbteste

Recovery Manager: Release 11.2.0.4.0 - Production on Sun Feb 2 22:26:02 2020

Copyright (c) 1982, 2011, Oracle and/or its affiliates.  All rights reserved.

connected to target database: DBPROD (DBID=1130215834)
connected to auxiliary database: DBTESTE (not mounted)

RMAN> run {
allocate channel ch1 type disk;
allocate auxiliary channel ch2 type disk;
SQL 'alter system switch logfile';
set newname for datafile 1 to '+DGDATA/dbteste/datafile/system01.dbf';
set newname for datafile 2 to '+DGDATA/dbteste/datafile/sysaux01.dbf';2> 3> 4> 5> 6>
set newname for datafile 3 to '+DGDATA/dbteste/datafile/undotbs01.dbf';
set newname for datafile 4 to '+DGDATA/dbteste/datafile/users01.dbf';
set newname for datafile 5 to '+DGDATA/dbteste/datafile/example.dbf';
set newname for datafile 6 to '+DGDATA/dbteste/datafile/data_teste01.dbf';
set newname for tempfile 1 to '+DGDATA/dbteste/datafile/temp01.dbf';
duplicate target database to dbteste from active database
SPFILE
    PARAMETER_VALUE_CONVERT='dbprod','dbteste'
    SET DB_NAME='dbteste'
    SET DB_UNIQUE_NAME='dbteste'
    SET CONTROL_FILES='+DGDATA'7> 8> 9> 10> 11> 12> 13> 14> 15> 16>
    set SGA_TARGET='1024M'
    set PGA_AGGREGATE_TARGET='512M'
    SET DIAGNOSTIC_DEST='/orabin01/app/oracle'
    SET DB_FILE_NAME_CONVERT='+DGDATA/dbprod/datafile/','+DGDATA/dbteste/datafile/', '+DGDATA/dbprod/tempfile/','+DGDATA/dbteste/tempfile/'
    SET LOG_FILE_NAME_CONVERT='+DGRECO/dbprod/onlinelog/','+DGRECO/dbteste/onlinelog/'
    SET LOG_ARCHIVE_FORMAT='dbteste_%t_%s_%r.arc'
    set LOG_ARCHIVE_DEST='/orabin01/dbteste/archive/';
}17> 18> 19> 20> 21> 22> 23> 24>

using target database control file instead of recovery catalog
allocated channel: ch1
channel ch1: SID=249 device type=DISK

allocated channel: ch2
channel ch2: SID=355 device type=DISK

sql statement: alter system switch logfile

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

executing command: SET NEWNAME

Starting Duplicate Db at 02-FEB-20

contents of Memory Script:
{
   backup as copy reuse
   targetfile  '+DGDATA/dbprod/spfiledbprod.ora' auxiliary format
 '+DGDATA/dbteste/spfiledbteste.ora'   ;
   sql clone "alter system set spfile= ''+DGDATA/dbteste/spfiledbteste.ora''";
}
executing Memory Script

Starting backup at 02-FEB-20
Finished backup at 02-FEB-20

sql statement: alter system set spfile= ''+DGDATA/dbteste/spfiledbteste.ora''

contents of Memory Script:
{
   sql clone "alter system set  db_name =
 ''DBTESTE'' comment=
 ''duplicate'' scope=spfile";
   sql clone "alter system set  db_unique_name =
 ''DBTESTE'' comment=
 ''duplicate'' scope=spfile";
   sql clone "alter system set  db_name =
 ''dbteste'' comment=
 '''' scope=spfile";
   sql clone "alter system set  db_unique_name =
 ''dbteste'' comment=
 '''' scope=spfile";
   sql clone "alter system set  CONTROL_FILES =
 ''+DGDATA'' comment=
 '''' scope=spfile";
   sql clone "alter system set  SGA_TARGET =
 1024M comment=
 '''' scope=spfile";
   sql clone "alter system set  PGA_AGGREGATE_TARGET =
 512M comment=
 '''' scope=spfile";
   sql clone "alter system set  DIAGNOSTIC_DEST =
 ''/orabin01/app/oracle'' comment=
 '''' scope=spfile";
   sql clone "alter system set  db_file_name_convert =
 ''+DGDATA/dbprod/datafile/'', ''+DGDATA/dbteste/datafile/'', ''+DGDATA/dbprod/tempfile/'', ''+DGDATA/dbteste/tempfile/'' comment=
 '''' scope=spfile";
   sql clone "alter system set  LOG_FILE_NAME_CONVERT =
 ''+DGRECO/dbprod/onlinelog/'', ''+DGRECO/dbteste/onlinelog/'' comment=
 '''' scope=spfile";
   sql clone "alter system set  LOG_ARCHIVE_FORMAT =
 ''dbteste_%t_%s_%r.arc'' comment=
 '''' scope=spfile";
   sql clone "alter system set  LOG_ARCHIVE_DEST =
 ''/orabin01/dbteste/archive/'' comment=
 '''' scope=spfile";
   shutdown clone immediate;
   startup clone nomount;
}
executing Memory Script

sql statement: alter system set  db_name =  ''DBTESTE'' comment= ''duplicate'' scope=spfile

sql statement: alter system set  db_unique_name =  ''DBTESTE'' comment= ''duplicate'' scope=spfile

sql statement: alter system set  db_name =  ''dbteste'' comment= '''' scope=spfile

sql statement: alter system set  db_unique_name =  ''dbteste'' comment= '''' scope=spfile

sql statement: alter system set  CONTROL_FILES =  ''+DGDATA'' comment= '''' scope=spfile

sql statement: alter system set  SGA_TARGET =  1024M comment= '''' scope=spfile

sql statement: alter system set  PGA_AGGREGATE_TARGET =  512M comment= '''' scope=spfile

sql statement: alter system set  DIAGNOSTIC_DEST =  ''/orabin01/app/oracle'' comment= '''' scope=spfile

sql statement: alter system set  db_file_name_convert =  ''+DGDATA/dbprod/datafile/'', ''+DGDATA/dbteste/datafile/'', ''+DGDATA/dbprod/tempfile/'', ''+DGDATA/dbteste/tempfile/'' comment= '''' scope=spfile

sql statement: alter system set  LOG_FILE_NAME_CONVERT =  ''+DGRECO/dbprod/onlinelog/'', ''+DGRECO/dbteste/onlinelog/'' comment= '''' scope=spfile

sql statement: alter system set  LOG_ARCHIVE_FORMAT =  ''dbteste_%t_%s_%r.arc'' comment= '''' scope=spfile

sql statement: alter system set  LOG_ARCHIVE_DEST =  ''/orabin01/dbteste/archive/'' comment= '''' scope=spfile

Oracle instance shut down

connected to auxiliary database (not started)
Oracle instance started

Total System Global Area    1603411968 bytes

Fixed Size                     2253664 bytes
Variable Size                520096928 bytes
Database Buffers            1073741824 bytes
Redo Buffers                   7319552 bytes
allocated channel: ch2
channel ch2: SID=123 device type=DISK

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
allocated channel: ch2
channel ch2: SID=123 device type=DISK

Starting backup at 02-FEB-20
channel ch1: starting datafile copy
copying current control file
output file name=/orabin01/app/oracle/product/11.2.0.4/dbhome_1/dbs/snapcf_dbprod.f tag=TAG20200202T222730 RECID=32 STAMP=1031351251
channel ch1: datafile copy complete, elapsed time: 00:00:03
Finished backup at 02-FEB-20

Starting restore at 02-FEB-20

channel ch2: copied control file copy
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
allocated channel: ch2
channel ch2: SID=123 device type=DISK

database mounted

contents of Memory Script:
{
   set newname for datafile  1 to
 "+DGDATA/dbteste/datafile/system01.dbf";
   set newname for datafile  2 to
 "+DGDATA/dbteste/datafile/sysaux01.dbf";
   set newname for datafile  3 to
 "+DGDATA/dbteste/datafile/undotbs01.dbf";
   set newname for datafile  4 to
 "+DGDATA/dbteste/datafile/users01.dbf";
   set newname for datafile  5 to
 "+DGDATA/dbteste/datafile/example.dbf";
   set newname for datafile  6 to
 "+DGDATA/dbteste/datafile/data_teste01.dbf";
   backup as copy reuse
   datafile  1 auxiliary format
 "+DGDATA/dbteste/datafile/system01.dbf"   datafile
 2 auxiliary format
 "+DGDATA/dbteste/datafile/sysaux01.dbf"   datafile
 3 auxiliary format
 "+DGDATA/dbteste/datafile/undotbs01.dbf"   datafile
 4 auxiliary format
 "+DGDATA/dbteste/datafile/users01.dbf"   datafile
 5 auxiliary format
 "+DGDATA/dbteste/datafile/example.dbf"   datafile
 6 auxiliary format
 "+DGDATA/dbteste/datafile/data_teste01.dbf"   ;
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
channel ch1: starting datafile copy
input datafile file number=00001 name=+DGDATA/dbprod/datafile/system.256.1031136205
output file name=+DGDATA/dbteste/datafile/system01.dbf tag=TAG20200202T222753
channel ch1: datafile copy complete, elapsed time: 00:00:35
channel ch1: starting datafile copy
input datafile file number=00002 name=+DGDATA/dbprod/datafile/sysaux.257.1031136207
output file name=+DGDATA/dbteste/datafile/sysaux01.dbf tag=TAG20200202T222753
channel ch1: datafile copy complete, elapsed time: 00:00:25
channel ch1: starting datafile copy
input datafile file number=00003 name=+DGDATA/dbprod/datafile/undotbs1.258.1031136207
output file name=+DGDATA/dbteste/datafile/undotbs01.dbf tag=TAG20200202T222753
channel ch1: datafile copy complete, elapsed time: 00:00:15
channel ch1: starting datafile copy
input datafile file number=00005 name=+DGDATA/dbprod/datafile/example.269.1031136307
output file name=+DGDATA/dbteste/datafile/example.dbf tag=TAG20200202T222753
channel ch1: datafile copy complete, elapsed time: 00:00:15
channel ch1: starting datafile copy
input datafile file number=00006 name=+DGDATA/dbprod/datafile/data_teste01.dbf
output file name=+DGDATA/dbteste/datafile/data_teste01.dbf tag=TAG20200202T222753
channel ch1: datafile copy complete, elapsed time: 00:00:15
channel ch1: starting datafile copy
input datafile file number=00004 name=+DGDATA/dbprod/datafile/users.259.1031136207
output file name=+DGDATA/dbteste/datafile/users01.dbf tag=TAG20200202T222753
channel ch1: datafile copy complete, elapsed time: 00:00:01
Finished backup at 02-FEB-20

sql statement: alter system archive log current

contents of Memory Script:
{
   backup as copy reuse
   archivelog like  "/orabin01/archive/dbprod_1_208_1031136284.arc" auxiliary format
 "+DGDATA"   archivelog like
 "/orabin01/archive/dbprod_1_212_1031136284.arc" auxiliary format
 "+DGDATA"   archivelog like
 "/orabin01/archive/dbprod_1_213_1031136284.arc" auxiliary format
 "+DGDATA"   archivelog like
 "/orabin01/archive/dbprod_1_215_1031136284.arc" auxiliary format
 "+DGDATA"   archivelog like
 "/orabin01/archive/dbprod_1_209_1031136284.arc" auxiliary format
 "+DGDATA"   archivelog like
 "/orabin01/archive/dbprod_1_214_1031136284.arc" auxiliary format
 "+DGDATA"   archivelog like
 "/orabin01/archive/dbprod_1_210_1031136284.arc" auxiliary format
 "+DGDATA"   archivelog like
 "/orabin01/archive/dbprod_1_211_1031136284.arc" auxiliary format
 "+DGDATA"   ;
   catalog clone start with  "+DGDATA";
   switch clone datafile all;
}
executing Memory Script

Starting backup at 02-FEB-20
specification does not match any archived log in the repository
specification does not match any archived log in the repository
specification does not match any archived log in the repository
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
}
executing Memory Script

sql statement: alter system set  db_name =  ''DBTESTE'' comment= ''Reset to original value by RMAN'' scope=spfile

sql statement: alter system reset  db_unique_name scope=spfile

Oracle instance shut down
released channel: ch1
RMAN-00571: ===========================================================
RMAN-00569: =============== ERROR MESSAGE STACK FOLLOWS ===============
RMAN-00571: ===========================================================
RMAN-03002: failure of Duplicate Db command at 02/02/2020 22:29:54
RMAN-05501: aborting duplication of target database
RMAN-03015: error occurred in stored script Memory Script
RMAN-06059: expected archived log not found, loss of archived log compromises recoverability
ORA-19625: error identifying file /orabin01/archive/dbprod_1_212_1031136284.arc
ORA-27037: unable to obtain file status
Linux-x86_64 Error: 2: No such file or directory
Additional information: 3

RMAN>
```