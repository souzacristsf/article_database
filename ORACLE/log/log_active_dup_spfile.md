
## Log do active duplicate database com a opção do SPFILE explicito no comando executado via RMAN.


```sql
[oracle@lab ~]$  rman target sys/891866@dbprod auxiliary sys/891866@dbteste

Recovery Manager: Release 11.2.0.4.0 - Production on Sat Feb 1 22:51:28 2020

Copyright (c) 1982, 2011, Oracle and/or its affiliates.  All rights reserved.

connected to target database: DBPROD (DBID=1130215834)
connected to auxiliary database: DBTESTE (not mounted)

RMAN> run {
allocate channel ch1 type disk;
allocate auxiliary channel ch2 type disk;
SQL 'alter system switch logfile';
duplicate target database to dbteste from active database
SPFILE
SET DB_NAME='dbteste'
SET DB_UNIQUE_NAME='dbteste'
SET CONTROL_FILES='+DGDATA'
set SGA_TARGET='1024M'
set PGA_AGGREGATE_TARGET='512M'
SET DIAGNOSTIC_DEST='/orabin01/app/oracle'
SET DB_FILE_NAME_CONVERT='+DGDATA/dbprod/datafile/','+DGDATA/dbteste/datafile/', '+DGDATA/dbprod/tempfile/','+DGDATA/dbteste/tempfile/'
SET LOG_FILE_NAME_CONVERT='+DGRECO/dbprod/onlinelog/','+DGRECO/dbteste/onlinelog/'
SET LOG_ARCHIVE_FORMAT='dbteste_%t_%s_%r.arc'
set LOG_ARCHIVE_DEST='/orabin01/dbteste/archive/';
}2> 3> 4> 5> 6> 7> 8> 9> 10> 11> 12> 13> 14> 15> 16> 17>

using target database control file instead of recovery catalog
allocated channel: ch1
channel ch1: SID=129 device type=DISK

allocated channel: ch2
channel ch2: SID=243 device type=DISK

sql statement: alter system switch logfile

Starting Duplicate Db at 01-FEB-20

contents of Memory Script:
{
   backup as copy reuse
   targetfile  '+DGDATA/dbprod/spfiledbprod.ora' auxiliary format
 '+DGDATA/dbteste/spfiledbteste.ora'   ;
   sql clone "alter system set spfile= ''+DGDATA/dbteste/spfiledbteste.ora''";
}
executing Memory Script

Starting backup at 01-FEB-20
Finished backup at 01-FEB-20

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

Total System Global Area    1068937216 bytes

Fixed Size                     2260088 bytes
Variable Size                381682568 bytes
Database Buffers             679477248 bytes
Redo Buffers                   5517312 bytes
allocated channel: ch2
channel ch2: SID=123 device type=DISK

contents of Memory Script:
{
   sql clone "alter system set  control_files =
  ''+DGDATA/dbteste/controlfile/current.299.1031266411'' comment=
 ''Set by RMAN'' scope=spfile";
   sql clone "alter system set  db_name =
 ''DBPROD'' comment=
 ''Modified by RMAN duplicate'' scope=spfile";
   sql clone "alter system set  db_unique_name =
 ''DBTESTE'' comment=
 ''Modified by RMAN duplicate'' scope=spfile";
   shutdown clone immediate;
   startup clone force nomount
   backup as copy current controlfile auxiliary format  '+DGDATA/dbteste/controlfile/current.300.1031266411';
   sql clone "alter system set  control_files =
  ''+DGDATA/dbteste/controlfile/current.300.1031266411'' comment=
 ''Set by RMAN'' scope=spfile";
   shutdown clone immediate;
   startup clone nomount;
   alter clone database mount;
}
executing Memory Script

sql statement: alter system set  control_files =   ''+DGDATA/dbteste/controlfile/current.299.1031266411'' comment= ''Set by RMAN'' scope=spfile

sql statement: alter system set  db_name =  ''DBPROD'' comment= ''Modified by RMAN duplicate'' scope=spfile

sql statement: alter system set  db_unique_name =  ''DBTESTE'' comment= ''Modified by RMAN duplicate'' scope=spfile

Oracle instance shut down

Oracle instance started

Total System Global Area    1068937216 bytes

Fixed Size                     2260088 bytes
Variable Size                381682568 bytes
Database Buffers             679477248 bytes
Redo Buffers                   5517312 bytes
allocated channel: ch2
channel ch2: SID=123 device type=DISK

Starting backup at 01-FEB-20
channel ch1: starting datafile copy
copying current control file
output file name=/orabin01/app/oracle/product/11.2.0.4/dbhome_1/dbs/snapcf_dbprod.f tag=TAG20200201T225341 RECID=18 STAMP=1031266421
channel ch1: datafile copy complete, elapsed time: 00:00:03
Finished backup at 01-FEB-20

sql statement: alter system set  control_files =   ''+DGDATA/dbteste/controlfile/current.300.1031266411'' comment= ''Set by RMAN'' scope=spfile

Oracle instance shut down

connected to auxiliary database (not started)
Oracle instance started

Total System Global Area    1068937216 bytes

Fixed Size                     2260088 bytes
Variable Size                381682568 bytes
Database Buffers             679477248 bytes
Redo Buffers                   5517312 bytes
allocated channel: ch2
channel ch2: SID=123 device type=DISK

database mounted
RMAN-05529: WARNING: DB_FILE_NAME_CONVERT resulted in invalid ASM names; names changed to disk group only.
Using previous duplicated file +DGDATA/dbteste/datafile/sysaux.292.1031260159 for datafile 2 with checkpoint SCN of 1141051
Using previous duplicated file +DGDATA/dbteste/datafile/undotbs1.295.1031260173 for datafile 3 with checkpoint SCN of 1141062
Using previous duplicated file +DGDATA/dbteste/datafile/users.296.1031260191 for datafile 4 with checkpoint SCN of 1141071

contents of Memory Script:
{
   set newname for datafile  1 to
 "+dgdata";
   set newname for datafile  2 to
 "+DGDATA/dbteste/datafile/sysaux.292.1031260159";
   set newname for datafile  3 to
 "+DGDATA/dbteste/datafile/undotbs1.295.1031260173";
   set newname for datafile  4 to
 "+DGDATA/dbteste/datafile/users.296.1031260191";
   set newname for datafile  5 to
 "+dgdata";
   set newname for datafile  6 to
 "+DGDATA/dbteste/datafile/data_teste01.dbf";
   backup as copy reuse
   datafile  1 auxiliary format
 "+dgdata"   datafile
 5 auxiliary format
 "+dgdata"   datafile
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

Starting backup at 01-FEB-20
channel ch1: starting datafile copy
input datafile file number=00001 name=+DGDATA/dbprod/datafile/system.256.1031136205
output file name=+DGDATA/dbteste/datafile/system.301.1031266443 tag=TAG20200201T225403
channel ch1: datafile copy complete, elapsed time: 00:00:35
channel ch1: starting datafile copy
input datafile file number=00005 name=+DGDATA/dbprod/datafile/example.269.1031136307
output file name=+DGDATA/dbteste/datafile/example.302.1031266479 tag=TAG20200201T225403
channel ch1: datafile copy complete, elapsed time: 00:00:15
channel ch1: starting datafile copy
input datafile file number=00006 name=+DGDATA/dbprod/datafile/data_teste01.dbf
output file name=+DGDATA/dbteste/datafile/data_teste01.dbf tag=TAG20200201T225403
channel ch1: datafile copy complete, elapsed time: 00:00:15
Finished backup at 01-FEB-20

sql statement: alter system archive log current

contents of Memory Script:
{
   backup as copy reuse
   archivelog like  "/orabin01/archive/dbprod_1_142_1031136284.arc" auxiliary format
 "/orabin01/dbteste/archive/dbteste_1_142_1031136284.arc"   archivelog like
 "/orabin01/archive/dbprod_1_140_1031136284.arc" auxiliary format
 "/orabin01/dbteste/archive/dbteste_1_140_1031136284.arc"   archivelog like
 "/orabin01/archive/dbprod_1_139_1031136284.arc" auxiliary format
 "/orabin01/dbteste/archive/dbteste_1_139_1031136284.arc"   archivelog like
 "/orabin01/archive/dbprod_1_141_1031136284.arc" auxiliary format
 "/orabin01/dbteste/archive/dbteste_1_141_1031136284.arc"   ;
   catalog clone archivelog  "/orabin01/dbteste/archive/dbteste_1_142_1031136284.arc";
   catalog clone archivelog  "/orabin01/dbteste/archive/dbteste_1_140_1031136284.arc";
   catalog clone archivelog  "/orabin01/dbteste/archive/dbteste_1_139_1031136284.arc";
   catalog clone archivelog  "/orabin01/dbteste/archive/dbteste_1_141_1031136284.arc";
   catalog clone datafilecopy  "+DGDATA/dbteste/datafile/sysaux.292.1031260159",
 "+DGDATA/dbteste/datafile/undotbs1.295.1031260173",
 "+DGDATA/dbteste/datafile/users.296.1031260191";
   switch clone datafile  2 to datafilecopy
 "+DGDATA/dbteste/datafile/sysaux.292.1031260159";
   switch clone datafile  3 to datafilecopy
 "+DGDATA/dbteste/datafile/undotbs1.295.1031260173";
   switch clone datafile  4 to datafilecopy
 "+DGDATA/dbteste/datafile/users.296.1031260191";
   switch clone datafile all;
}
executing Memory Script

Starting backup at 01-FEB-20
channel ch1: starting archived log copy
input archived log thread=1 sequence=142 RECID=32 STAMP=1031266509
output file name=/orabin01/dbteste/archive/dbteste_1_142_1031136284.arc RECID=0 STAMP=0
channel ch1: archived log copy complete, elapsed time: 00:00:01
channel ch1: starting archived log copy
input archived log thread=1 sequence=140 RECID=30 STAMP=1031263993
output file name=/orabin01/dbteste/archive/dbteste_1_140_1031136284.arc RECID=0 STAMP=0
channel ch1: archived log copy complete, elapsed time: 00:00:01
channel ch1: starting archived log copy
input archived log thread=1 sequence=139 RECID=29 STAMP=1031260466
output file name=/orabin01/dbteste/archive/dbteste_1_139_1031136284.arc RECID=0 STAMP=0
channel ch1: archived log copy complete, elapsed time: 00:00:01
channel ch1: starting archived log copy
input archived log thread=1 sequence=141 RECID=31 STAMP=1031266392
output file name=/orabin01/dbteste/archive/dbteste_1_141_1031136284.arc RECID=0 STAMP=0
channel ch1: archived log copy complete, elapsed time: 00:00:01
Finished backup at 01-FEB-20

cataloged archived log
archived log file name=/orabin01/dbteste/archive/dbteste_1_142_1031136284.arc RECID=32 STAMP=1031266514

cataloged archived log
archived log file name=/orabin01/dbteste/archive/dbteste_1_140_1031136284.arc RECID=33 STAMP=1031266514

cataloged archived log
archived log file name=/orabin01/dbteste/archive/dbteste_1_139_1031136284.arc RECID=34 STAMP=1031266514

cataloged archived log
archived log file name=/orabin01/dbteste/archive/dbteste_1_141_1031136284.arc RECID=35 STAMP=1031266514

cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/sysaux.292.1031260159 RECID=18 STAMP=1031266514
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/undotbs1.295.1031260173 RECID=19 STAMP=1031266514
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/users.296.1031260191 RECID=20 STAMP=1031266514

datafile 2 switched to datafile copy
input datafile copy RECID=18 STAMP=1031266514 file name=+DGDATA/dbteste/datafile/sysaux.292.1031260159

datafile 3 switched to datafile copy
input datafile copy RECID=19 STAMP=1031266514 file name=+DGDATA/dbteste/datafile/undotbs1.295.1031260173

datafile 4 switched to datafile copy
input datafile copy RECID=20 STAMP=1031266514 file name=+DGDATA/dbteste/datafile/users.296.1031260191

datafile 1 switched to datafile copy
input datafile copy RECID=21 STAMP=1031266515 file name=+DGDATA/dbteste/datafile/system.301.1031266443
datafile 5 switched to datafile copy
input datafile copy RECID=22 STAMP=1031266515 file name=+DGDATA/dbteste/datafile/example.302.1031266479
datafile 6 switched to datafile copy
input datafile copy RECID=23 STAMP=1031266515 file name=+DGDATA/dbteste/datafile/data_teste01.dbf

contents of Memory Script:
{
   set until scn  1147031;
   recover
   clone database
    delete archivelog
   ;
}
executing Memory Script

executing command: SET until clause

Starting recover at 01-FEB-20

starting media recovery

archived log for thread 1 with sequence 139 is already on disk as file /orabin01/dbteste/archive/dbteste_1_139_1031136284.arc
archived log for thread 1 with sequence 140 is already on disk as file /orabin01/dbteste/archive/dbteste_1_140_1031136284.arc
archived log for thread 1 with sequence 141 is already on disk as file /orabin01/dbteste/archive/dbteste_1_141_1031136284.arc
archived log for thread 1 with sequence 142 is already on disk as file /orabin01/dbteste/archive/dbteste_1_142_1031136284.arc
archived log file name=/orabin01/dbteste/archive/dbteste_1_139_1031136284.arc thread=1 sequence=139
archived log file name=/orabin01/dbteste/archive/dbteste_1_140_1031136284.arc thread=1 sequence=140
archived log file name=/orabin01/dbteste/archive/dbteste_1_141_1031136284.arc thread=1 sequence=141
archived log file name=/orabin01/dbteste/archive/dbteste_1_142_1031136284.arc thread=1 sequence=142
media recovery complete, elapsed time: 00:00:02
Finished recover at 01-FEB-20
Oracle instance started

Total System Global Area    1068937216 bytes

Fixed Size                     2260088 bytes
Variable Size                381682568 bytes
Database Buffers             679477248 bytes
Redo Buffers                   5517312 bytes

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

Total System Global Area    1068937216 bytes

Fixed Size                     2260088 bytes
Variable Size                381682568 bytes
Database Buffers             679477248 bytes
Redo Buffers                   5517312 bytes
allocated channel: ch2
channel ch2: SID=123 device type=DISK
sql statement: CREATE CONTROLFILE REUSE SET DATABASE "DBTESTE" RESETLOGS ARCHIVELOG
  MAXLOGFILES     16
  MAXLOGMEMBERS      3
  MAXDATAFILES      100
  MAXINSTANCES     8
  MAXLOGHISTORY      292
 LOGFILE
  GROUP   1 ( '+DGRECO/dbteste/onlinelog/redo1_01.log', '+DGRECO/dbteste/onlinelog/redo1_02.log' ) SIZE 50 M  REUSE,
  GROUP   2 ( '+DGRECO/dbteste/onlinelog/redo2_01.log', '+DGRECO/dbteste/onlinelog/redo2_02.log' ) SIZE 50 M  REUSE,
  GROUP   3 ( '+DGRECO/dbteste/onlinelog/redo3_01.log', '+DGRECO/dbteste/onlinelog/redo3_02.log' ) SIZE 50 M  REUSE
 DATAFILE
  '+DGDATA/dbteste/datafile/system.301.1031266443'
 CHARACTER SET AL32UTF8


contents of Memory Script:
{
   set newname for tempfile  1 to
 "+dgdata";
   switch clone tempfile all;
   catalog clone datafilecopy  "+DGDATA/dbteste/datafile/sysaux.292.1031260159",
 "+DGDATA/dbteste/datafile/undotbs1.295.1031260173",
 "+DGDATA/dbteste/datafile/users.296.1031260191",
 "+DGDATA/dbteste/datafile/example.302.1031266479",
 "+DGDATA/dbteste/datafile/data_teste01.dbf";
   switch clone datafile all;
}
executing Memory Script

executing command: SET NEWNAME

renamed tempfile 1 to +dgdata in control file

cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/sysaux.292.1031260159 RECID=1 STAMP=1031266543
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/undotbs1.295.1031260173 RECID=2 STAMP=1031266543
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/users.296.1031260191 RECID=3 STAMP=1031266543
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/example.302.1031266479 RECID=4 STAMP=1031266543
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/data_teste01.dbf RECID=5 STAMP=1031266543

datafile 5 switched to datafile copy
input datafile copy RECID=4 STAMP=1031266543 file name=+DGDATA/dbteste/datafile/example.302.1031266479
datafile 6 switched to datafile copy
input datafile copy RECID=5 STAMP=1031266543 file name=+DGDATA/dbteste/datafile/data_teste01.dbf
datafile 2 switched to datafile copy
input datafile copy RECID=6 STAMP=1031266543 file name=+DGDATA/dbteste/datafile/sysaux.292.1031260159
datafile 3 switched to datafile copy
input datafile copy RECID=7 STAMP=1031266543 file name=+DGDATA/dbteste/datafile/undotbs1.295.1031260173
datafile 4 switched to datafile copy
input datafile copy RECID=8 STAMP=1031266543 file name=+DGDATA/dbteste/datafile/users.296.1031260191

contents of Memory Script:
{
   Alter clone database open resetlogs;
}
executing Memory Script

database opened
Finished Duplicate Db at 01-FEB-20
released channel: ch1
released channel: ch2
```