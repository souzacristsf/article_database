
## Log do active duplicate database utilizando OMF.

```sql
[oracle@lab ~]$  rman target sys/891866@dbprod auxiliary sys/891866@dbteste

Recovery Manager: Release 11.2.0.4.0 - Production on Sat Feb 1 23:55:37 2020

Copyright (c) 1982, 2011, Oracle and/or its affiliates.  All rights reserved.

connected to target database: DBPROD (DBID=1130215834)
connected to auxiliary database: DBTESTE (not mounted)

RMAN> run {
allocate channel aux1 type disk;
allocate auxiliary channel aux2 type disk;
SQL 'alter system switch logfile';
duplicate target database to dbteste from active database;
}2> 3> 4> 5> 6>

using target database control file instead of recovery catalog
allocated channel: aux1
channel aux1: SID=366 device type=DISK

allocated channel: aux2
channel aux2: SID=243 device type=DISK

sql statement: alter system switch logfile

Starting Duplicate Db at 01-FEB-20

contents of Memory Script:
{
   sql clone "alter system set  control_files =
  ''+DGDATA/dbteste/controlfile/current.300.1031266411'' comment=
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

sql statement: alter system set  control_files =   ''+DGDATA/dbteste/controlfile/current.300.1031266411'' comment= ''Set by RMAN'' scope=spfile

sql statement: alter system set  db_name =  ''DBPROD'' comment= ''Modified by RMAN duplicate'' scope=spfile

sql statement: alter system set  db_unique_name =  ''DBTESTE'' comment= ''Modified by RMAN duplicate'' scope=spfile

Oracle instance shut down

Oracle instance started

Total System Global Area    1068937216 bytes

Fixed Size                     2260088 bytes
Variable Size                381682568 bytes
Database Buffers             679477248 bytes
Redo Buffers                   5517312 bytes
allocated channel: aux2
channel aux2: SID=123 device type=DISK

Starting backup at 01-FEB-20
channel aux1: starting datafile copy
copying current control file
output file name=/orabin01/app/oracle/product/11.2.0.4/dbhome_1/dbs/snapcf_dbprod.f tag=TAG20200201T235556 RECID=19 STAMP=1031270157
channel aux1: datafile copy complete, elapsed time: 00:00:03
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
allocated channel: aux2
channel aux2: SID=123 device type=DISK

database mounted

contents of Memory Script:
{
   set newname for clone datafile  1 to new;
   set newname for clone datafile  2 to new;
   set newname for clone datafile  3 to new;
   set newname for clone datafile  4 to new;
   set newname for clone datafile  5 to new;
   set newname for clone datafile  6 to new;
   backup as copy reuse
   datafile  1 auxiliary format new
   datafile  2 auxiliary format new
   datafile  3 auxiliary format new
   datafile  4 auxiliary format new
   datafile  5 auxiliary format new
   datafile  6 auxiliary format new
   ;
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
channel aux1: starting datafile copy
input datafile file number=00001 name=+DGDATA/dbprod/datafile/system.256.1031136205
output file name=+DGDATA/dbteste/datafile/system.305.1031270177 tag=TAG20200201T235616
channel aux1: datafile copy complete, elapsed time: 00:00:46
channel aux1: starting datafile copy
input datafile file number=00002 name=+DGDATA/dbprod/datafile/sysaux.257.1031136207
output file name=+DGDATA/dbteste/datafile/sysaux.306.1031270223 tag=TAG20200201T235616
channel aux1: datafile copy complete, elapsed time: 00:00:25
channel aux1: starting datafile copy
input datafile file number=00003 name=+DGDATA/dbprod/datafile/undotbs1.258.1031136207
output file name=+DGDATA/dbteste/datafile/undotbs1.307.1031270247 tag=TAG20200201T235616
channel aux1: datafile copy complete, elapsed time: 00:00:15
channel aux1: starting datafile copy
input datafile file number=00005 name=+DGDATA/dbprod/datafile/example.269.1031136307
output file name=+DGDATA/dbteste/datafile/example.308.1031270263 tag=TAG20200201T235616
channel aux1: datafile copy complete, elapsed time: 00:00:15
channel aux1: starting datafile copy
input datafile file number=00006 name=+DGDATA/dbprod/datafile/data_teste01.dbf
output file name=+DGDATA/dbteste/datafile/teste.309.1031270277 tag=TAG20200201T235616
channel aux1: datafile copy complete, elapsed time: 00:00:15
channel aux1: starting datafile copy
input datafile file number=00004 name=+DGDATA/dbprod/datafile/users.259.1031136207
output file name=+DGDATA/dbteste/datafile/users.310.1031270293 tag=TAG20200201T235616
channel aux1: datafile copy complete, elapsed time: 00:00:01
Finished backup at 01-FEB-20

sql statement: alter system archive log current

contents of Memory Script:
{
   backup as copy reuse
   archivelog like  "/orabin01/archive/dbprod_1_149_1031136284.arc" auxiliary format
 "/orabin01/dbteste/archive/dbteste_1_149_1031136284.arc"   ;
   catalog clone archivelog  "/orabin01/dbteste/archive/dbteste_1_149_1031136284.arc";
   switch clone datafile all;
}
executing Memory Script

Starting backup at 01-FEB-20
channel aux1: starting archived log copy
input archived log thread=1 sequence=149 RECID=39 STAMP=1031270294
output file name=/orabin01/dbteste/archive/dbteste_1_149_1031136284.arc RECID=0 STAMP=0
channel aux1: archived log copy complete, elapsed time: 00:00:01
Finished backup at 01-FEB-20

cataloged archived log
archived log file name=/orabin01/dbteste/archive/dbteste_1_149_1031136284.arc RECID=39 STAMP=1031270295

datafile 1 switched to datafile copy
input datafile copy RECID=19 STAMP=1031270296 file name=+DGDATA/dbteste/datafile/system.305.1031270177
datafile 2 switched to datafile copy
input datafile copy RECID=20 STAMP=1031270296 file name=+DGDATA/dbteste/datafile/sysaux.306.1031270223
datafile 3 switched to datafile copy
input datafile copy RECID=21 STAMP=1031270296 file name=+DGDATA/dbteste/datafile/undotbs1.307.1031270247
datafile 4 switched to datafile copy
input datafile copy RECID=22 STAMP=1031270296 file name=+DGDATA/dbteste/datafile/users.310.1031270293
datafile 5 switched to datafile copy
input datafile copy RECID=23 STAMP=1031270296 file name=+DGDATA/dbteste/datafile/example.308.1031270263
datafile 6 switched to datafile copy
input datafile copy RECID=24 STAMP=1031270296 file name=+DGDATA/dbteste/datafile/teste.309.1031270277

contents of Memory Script:
{
   set until scn  1169217;
   recover
   clone database
    delete archivelog
   ;
}
executing Memory Script

executing command: SET until clause

Starting recover at 01-FEB-20

starting media recovery

archived log for thread 1 with sequence 149 is already on disk as file /orabin01/dbteste/archive/dbteste_1_149_1031136284.arc
archived log file name=/orabin01/dbteste/archive/dbteste_1_149_1031136284.arc thread=1 sequence=149
media recovery complete, elapsed time: 00:00:01
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
  '+DGDATA/dbteste/datafile/system.305.1031270177'
 CHARACTER SET AL32UTF8


contents of Memory Script:
{
   set newname for clone tempfile  1 to new;
   switch clone tempfile all;
   catalog clone datafilecopy  "+DGDATA/dbteste/datafile/sysaux.306.1031270223",
 "+DGDATA/dbteste/datafile/undotbs1.307.1031270247",
 "+DGDATA/dbteste/datafile/users.310.1031270293",
 "+DGDATA/dbteste/datafile/example.308.1031270263",
 "+DGDATA/dbteste/datafile/teste.309.1031270277";
   switch clone datafile all;
}
executing Memory Script

executing command: SET NEWNAME

renamed tempfile 1 to +DGDATA in control file

cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/sysaux.306.1031270223 RECID=1 STAMP=1031270321
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/undotbs1.307.1031270247 RECID=2 STAMP=1031270321
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/users.310.1031270293 RECID=3 STAMP=1031270321
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/example.308.1031270263 RECID=4 STAMP=1031270321
cataloged datafile copy
datafile copy file name=+DGDATA/dbteste/datafile/teste.309.1031270277 RECID=5 STAMP=1031270321

datafile 2 switched to datafile copy
input datafile copy RECID=1 STAMP=1031270321 file name=+DGDATA/dbteste/datafile/sysaux.306.1031270223
datafile 3 switched to datafile copy
input datafile copy RECID=2 STAMP=1031270321 file name=+DGDATA/dbteste/datafile/undotbs1.307.1031270247
datafile 4 switched to datafile copy
input datafile copy RECID=3 STAMP=1031270321 file name=+DGDATA/dbteste/datafile/users.310.1031270293
datafile 5 switched to datafile copy
input datafile copy RECID=4 STAMP=1031270321 file name=+DGDATA/dbteste/datafile/example.308.1031270263
datafile 6 switched to datafile copy
input datafile copy RECID=5 STAMP=1031270321 file name=+DGDATA/dbteste/datafile/teste.309.1031270277

contents of Memory Script:
{
   Alter clone database open resetlogs;
}
executing Memory Script

database opened
Finished Duplicate Db at 01-FEB-20
released channel: aux1
released channel: aux2

RMAN>
```