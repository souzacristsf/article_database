
## Log do Active Duplicate Database com SET NEWNAME para mapear cada datafiles existem no ambiente origem/target para o ambiente destino/auxiliar e com a opção LOGFILE.

```sql
[oracle@lab ~]$  rman target sys/891866@dbprod auxiliary sys/891866@dbteste

Recovery Manager: Release 11.2.0.4.0 - Production on Sun Feb 2 17:03:25 2020

Copyright (c) 1982, 2011, Oracle and/or its affiliates.  All rights reserved.

connected to target database: DBPROD (DBID=1130215834)
connected to auxiliary database: DBTESTE (not mounted)

RMAN> run {
allocate channel ch1 type disk;
allocate auxiliary channel ch2 type disk;
SQL 'alter system switch logfile';
set newname for datafile 1 to '+DGDATA/dbteste/datafile/system01.dbf';
set newname for datafile 2 to '+DGDATA/dbteste/datafile/sysaux01.dbf';
set newname for datafile 3 to '+DGDATA/dbteste/datafile/undotbs01.dbf';
set newname for datafile 4 to '+DGDATA/dbteste/datafile/users01.dbf';
set newname for datafile 5 to '+DGDATA/dbteste/datafile/example.dbf';
set newname for datafile 6 to '+DGDATA/dbte2> 3> 4> 5> 6> 7> 8> 9> 10> ste/datafile/data_teste01.dbf';
set newname for tempfile 1 to '+DGDATA/dbteste/datafile/temp01.dbf';
duplicate target database to dbteste from active database
LOGFILE
      GROUP 1 ('+DGRECO/dbteste/onlinelog/redo1_01.log',
               '+DGRECO/dbteste/onlinelog/redo1_02.log') SIZE 50M REUSE,
      GROUP 2 ('+DGRECO/dbteste/onlinelog/redo2_01.log',
               '+DGRECO/dbteste/onlinelog/redo2_02.log') SIZE 50M REUSE,
          GROUP 3 ('+DGRECO/dbteste/onlinelog/redo3_01.log',
               '+DGRECO/dbteste/onlinelog/redo3_02.log') SIZE 50M REUSE;
}11> 12> 13> 14> 15> 16> 17> 18> 19> 20>

using target database control file instead of recovery catalog
allocated channel: ch1
channel ch1: SID=15 device type=DISK

allocated channel: ch2
channel ch2: SID=243 device type=DISK

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
output file name=/orabin01/app/oracle/product/11.2.0.4/dbhome_1/dbs/snapcf_dbprod.f tag=TAG20200202T170348 RECID=28 STAMP=1031331829
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
output file name=+DGDATA/dbteste/datafile/system01.dbf tag=TAG20200202T170410
channel ch1: datafile copy complete, elapsed time: 00:00:35
channel ch1: starting datafile copy
input datafile file number=00002 name=+DGDATA/dbprod/datafile/sysaux.257.1031136207
output file name=+DGDATA/dbteste/datafile/sysaux01.dbf tag=TAG20200202T170410
channel ch1: datafile copy complete, elapsed time: 00:00:15
channel ch1: starting datafile copy
input datafile file number=00003 name=+DGDATA/dbprod/datafile/undotbs1.258.1031136207
output file name=+DGDATA/dbteste/datafile/undotbs01.dbf tag=TAG20200202T170410
channel ch1: datafile copy complete, elapsed time: 00:00:15
channel ch1: starting datafile copy
input datafile file number=00005 name=+DGDATA/dbprod/datafile/example.269.1031136307
output file name=+DGDATA/dbteste/datafile/example.dbf tag=TAG20200202T170410
channel ch1: datafile copy complete, elapsed time: 00:00:15
channel ch1: starting datafile copy
input datafile file number=00006 name=+DGDATA/dbprod/datafile/data_teste01.dbf
output file name=+DGDATA/dbteste/datafile/data_teste01.dbf tag=TAG20200202T170410
channel ch1: datafile copy complete, elapsed time: 00:00:07
channel ch1: starting datafile copy
input datafile file number=00004 name=+DGDATA/dbprod/datafile/users.259.1031136207
output file name=+DGDATA/dbteste/datafile/users01.dbf tag=TAG20200202T170410
channel ch1: datafile copy complete, elapsed time: 00:00:01
Finished backup at 02-FEB-20

sql statement: alter system archive log current

contents of Memory Script:
{
   backup as copy reuse
   archivelog like  "/orabin01/archive/dbprod_1_168_1031136284.arc" auxiliary format
 "+DGDATA"   ;
   catalog clone start with  "+DGDATA";
   switch clone datafile all;
}
executing Memory Script

Starting backup at 02-FEB-20
channel ch1: starting archived log copy
input archived log thread=1 sequence=168 RECID=60 STAMP=1031331940
output file name=+DGDATA/dbteste/archivelog/2020_02_02/thread_1_seq_168.301.1031331941 RECID=0 STAMP=0
channel ch1: archived log copy complete, elapsed time: 00:00:01
Finished backup at 02-FEB-20

searching for all files that match the pattern +DGDATA

datafile 1 switched to datafile copy
input datafile copy RECID=43 STAMP=1031331944 file name=+DGDATA/dbteste/datafile/system01.dbf
datafile 2 switched to datafile copy
input datafile copy RECID=44 STAMP=1031331944 file name=+DGDATA/dbteste/datafile/sysaux01.dbf
datafile 3 switched to datafile copy
input datafile copy RECID=45 STAMP=1031331944 file name=+DGDATA/dbteste/datafile/undotbs01.dbf
datafile 4 switched to datafile copy
input datafile copy RECID=46 STAMP=1031331944 file name=+DGDATA/dbteste/datafile/users01.dbf
datafile 5 switched to datafile copy
input datafile copy RECID=47 STAMP=1031331944 file name=+DGDATA/dbteste/datafile/example.dbf
datafile 6 switched to datafile copy
input datafile copy RECID=48 STAMP=1031331944 file name=+DGDATA/dbteste/datafile/data_teste01.dbf

contents of Memory Script:
{
   set until scn  1224328;
   recover
   clone database
    delete archivelog
   ;
}
executing Memory Script

executing command: SET until clause

Starting recover at 02-FEB-20

starting media recovery

archived log for thread 1 with sequence 168 is already on disk as file +DGDATA/dbteste/archivelog/2020_02_02/thread_1_seq_168.301.1031331941
archived log file name=+DGDATA/dbteste/archivelog/2020_02_02/thread_1_seq_168.301.1031331941 thread=1 sequence=168
media recovery complete, elapsed time: 00:00:01
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
allocated channel: ch2
channel ch2: SID=124 device type=DISK
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
  '+DGDATA/dbteste/datafile/system01.dbf'
 CHARACTER SET AL32UTF8


contents of Memory Script:
{
   set newname for tempfile  1 to
 "+DGDATA/dbteste/datafile/temp01.dbf";
   switch clone tempfile all;
   catalog clone datafilecopy  "+DGDATA/dbteste/datafile/sysaux01.dbf",
 "+DGDATA/dbteste/datafile/undotbs01.dbf",
 "+DGDATA/dbteste/datafile/users01.dbf",
 "+DGDATA/dbteste/datafile/example.dbf",
 "+DGDATA/dbteste/datafile/data_teste01.dbf";
   switch clone datafile all;
}
executing Memory Script

executing command: SET NEWNAME

renamed tempfile 1 to +DGDATA/dbteste/datafile/temp01.dbf in control file

cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/sysaux01.dbf RECID=1 STAMP=1031331964
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/undotbs01.dbf RECID=2 STAMP=1031331964
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/users01.dbf RECID=3 STAMP=1031331964
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/example.dbf RECID=4 STAMP=1031331964
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/data_teste01.dbf RECID=5 STAMP=1031331964

datafile 2 switched to datafile copy
input datafile copy RECID=1 STAMP=1031331964 file name=+DGDATA/dbteste/datafile/sysaux01.dbf
datafile 3 switched to datafile copy
input datafile copy RECID=2 STAMP=1031331964 file name=+DGDATA/dbteste/datafile/undotbs01.dbf
datafile 4 switched to datafile copy
input datafile copy RECID=3 STAMP=1031331964 file name=+DGDATA/dbteste/datafile/users01.dbf
datafile 5 switched to datafile copy
input datafile copy RECID=4 STAMP=1031331964 file name=+DGDATA/dbteste/datafile/example.dbf
datafile 6 switched to datafile copy
input datafile copy RECID=5 STAMP=1031331964 file name=+DGDATA/dbteste/datafile/data_teste01.dbf

contents of Memory Script:
{
   Alter clone database open resetlogs;
}
executing Memory Script

database opened
Finished Duplicate Db at 02-FEB-20
released channel: ch1
released channel: ch2

RMAN> [oracle@lab ~]$  rman target sys/891866@dbprod auxiliary sys/891866@dbteste

```