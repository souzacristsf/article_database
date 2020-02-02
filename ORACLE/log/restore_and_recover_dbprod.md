
## Log do procedimento realizado para o restore e recover da instância DBPROD.

Registro do log do RMAN para recuperação do banco de dados de produção DBPROD.

```sql
RMAN> restore database;

Starting restore at 02-FEB-20
using channel ORA_DISK_1

channel ORA_DISK_1: starting datafile backup set restore
channel ORA_DISK_1: specifying datafile(s) to restore from backup set
channel ORA_DISK_1: restoring datafile 00001 to +DGDATA/dbprod/datafile/system.256.1031136205
channel ORA_DISK_1: restoring datafile 00002 to +DGDATA/dbprod/datafile/sysaux.257.1031136207
channel ORA_DISK_1: restoring datafile 00003 to +DGDATA/dbprod/datafile/undotbs1.258.1031136207
channel ORA_DISK_1: restoring datafile 00004 to +DGDATA/dbprod/datafile/users.259.1031136207
channel ORA_DISK_1: restoring datafile 00005 to +DGDATA/dbprod/datafile/example.269.1031136307
channel ORA_DISK_1: restoring datafile 00006 to +DGDATA/dbprod/datafile/data_teste01.dbf
channel ORA_DISK_1: reading from backup piece /orabin01/backup/dbprod/fisico/files/df_DBPROD_1_1_1031157261.dbf
channel ORA_DISK_1: piece handle=/orabin01/backup/dbprod/fisico/files/df_DBPROD_1_1_1031157261.dbf tag=BACKUPDATABASEFULLDIARIO
channel ORA_DISK_1: restored backup piece 1
channel ORA_DISK_1: restore complete, elapsed time: 00:01:25
Finished restore at 02-FEB-20

RMAN> recover database;

Starting recover at 02-FEB-20
using channel ORA_DISK_1

starting media recovery

archived log for thread 1 with sequence 113 is already on disk as file +DGDATA/dbprod/archivelog/2020_02_01/thread_1_seq_113.282.1031221317
archived log for thread 1 with sequence 114 is already on disk as file /orabin01/archive/1_114_1031136284.dbf
archived log for thread 1 with sequence 115 is already on disk as file /orabin01/archive/1_115_1031136284.dbf
archived log for thread 1 with sequence 116 is already on disk as file /orabin01/archive/1_116_1031136284.dbf
archived log for thread 1 with sequence 117 is already on disk as file /orabin01/archive/1_117_1031136284.dbf
archived log for thread 1 with sequence 118 is already on disk as file /orabin01/archive/dbprod_1_118_1031136284.arc
archived log for thread 1 with sequence 119 is already on disk as file /orabin01/archive/dbprod_1_119_1031136284.arc
archived log for thread 1 with sequence 120 is already on disk as file /orabin01/archive/dbprod_1_120_1031136284.arc
archived log for thread 1 with sequence 121 is already on disk as file /orabin01/archive/dbprod_1_121_1031136284.arc
archived log for thread 1 with sequence 122 is already on disk as file /orabin01/archive/dbprod_1_122_1031136284.arc
archived log for thread 1 with sequence 123 is already on disk as file /orabin01/archive/dbprod_1_123_1031136284.arc
archived log for thread 1 with sequence 124 is already on disk as file /orabin01/archive/dbprod_1_124_1031136284.arc
archived log for thread 1 with sequence 125 is already on disk as file /orabin01/archive/dbprod_1_125_1031136284.arc
archived log for thread 1 with sequence 126 is already on disk as file /orabin01/archive/dbprod_1_126_1031136284.arc
archived log for thread 1 with sequence 127 is already on disk as file /orabin01/archive/dbprod_1_127_1031136284.arc
archived log for thread 1 with sequence 128 is already on disk as file /orabin01/archive/dbprod_1_128_1031136284.arc
archived log for thread 1 with sequence 129 is already on disk as file /orabin01/archive/dbprod_1_129_1031136284.arc
archived log for thread 1 with sequence 130 is already on disk as file /orabin01/archive/dbprod_1_130_1031136284.arc
archived log for thread 1 with sequence 131 is already on disk as file /orabin01/archive/dbprod_1_131_1031136284.arc
archived log for thread 1 with sequence 132 is already on disk as file /orabin01/archive/dbprod_1_132_1031136284.arc
archived log for thread 1 with sequence 133 is already on disk as file /orabin01/archive/dbprod_1_133_1031136284.arc
archived log for thread 1 with sequence 134 is already on disk as file /orabin01/archive/dbprod_1_134_1031136284.arc
archived log for thread 1 with sequence 135 is already on disk as file /orabin01/archive/dbprod_1_135_1031136284.arc
archived log for thread 1 with sequence 136 is already on disk as file /orabin01/archive/dbprod_1_136_1031136284.arc
archived log for thread 1 with sequence 137 is already on disk as file /orabin01/archive/dbprod_1_137_1031136284.arc
archived log for thread 1 with sequence 138 is already on disk as file /orabin01/archive/dbprod_1_138_1031136284.arc
archived log for thread 1 with sequence 139 is already on disk as file /orabin01/archive/dbprod_1_139_1031136284.arc
archived log for thread 1 with sequence 140 is already on disk as file /orabin01/archive/dbprod_1_140_1031136284.arc
archived log for thread 1 with sequence 141 is already on disk as file /orabin01/archive/dbprod_1_141_1031136284.arc
archived log for thread 1 with sequence 142 is already on disk as file /orabin01/archive/dbprod_1_142_1031136284.arc
archived log for thread 1 with sequence 143 is already on disk as file /orabin01/archive/dbprod_1_143_1031136284.arc
archived log for thread 1 with sequence 144 is already on disk as file /orabin01/archive/dbprod_1_144_1031136284.arc
archived log for thread 1 with sequence 145 is already on disk as file /orabin01/archive/dbprod_1_145_1031136284.arc
archived log for thread 1 with sequence 146 is already on disk as file /orabin01/archive/dbprod_1_146_1031136284.arc
archived log for thread 1 with sequence 147 is already on disk as file /orabin01/archive/dbprod_1_147_1031136284.arc
archived log for thread 1 with sequence 148 is already on disk as file /orabin01/archive/dbprod_1_148_1031136284.arc
archived log for thread 1 with sequence 149 is already on disk as file /orabin01/archive/dbprod_1_149_1031136284.arc
archived log for thread 1 with sequence 150 is already on disk as file /orabin01/archive/dbprod_1_150_1031136284.arc
archived log for thread 1 with sequence 151 is already on disk as file /orabin01/archive/dbprod_1_151_1031136284.arc
archived log for thread 1 with sequence 152 is already on disk as file /orabin01/archive/dbprod_1_152_1031136284.arc
archived log for thread 1 with sequence 153 is already on disk as file /orabin01/archive/dbprod_1_153_1031136284.arc
archived log for thread 1 with sequence 154 is already on disk as file /orabin01/archive/dbprod_1_154_1031136284.arc
archived log for thread 1 with sequence 155 is already on disk as file /orabin01/archive/dbprod_1_155_1031136284.arc
channel ORA_DISK_1: starting archived log restore to default destination
channel ORA_DISK_1: restoring archived log
archived log thread=1 sequence=111
channel ORA_DISK_1: restoring archived log
archived log thread=1 sequence=112
channel ORA_DISK_1: reading from backup piece /orabin01/backup/dbprod/fisico/files/arch_DBPROD_3_1_1031157340.arc
channel ORA_DISK_1: piece handle=/orabin01/backup/dbprod/fisico/files/arch_DBPROD_3_1_1031157340.arc tag=BACKUPARCHIVELOGDIARIO
channel ORA_DISK_1: restored backup piece 1
channel ORA_DISK_1: restore complete, elapsed time: 00:00:01
archived log file name=/orabin01/archive/dbprod_1_111_1031136284.arc thread=1 sequence=111
archived log file name=/orabin01/archive/dbprod_1_112_1031136284.arc thread=1 sequence=112
archived log file name=+DGDATA/dbprod/archivelog/2020_02_01/thread_1_seq_113.282.1031221317 thread=1 sequence=113
archived log file name=/orabin01/archive/1_114_1031136284.dbf thread=1 sequence=114
archived log file name=/orabin01/archive/1_115_1031136284.dbf thread=1 sequence=115
archived log file name=/orabin01/archive/1_116_1031136284.dbf thread=1 sequence=116
archived log file name=/orabin01/archive/1_117_1031136284.dbf thread=1 sequence=117
archived log file name=/orabin01/archive/dbprod_1_118_1031136284.arc thread=1 sequence=118
archived log file name=/orabin01/archive/dbprod_1_119_1031136284.arc thread=1 sequence=119
archived log file name=/orabin01/archive/dbprod_1_120_1031136284.arc thread=1 sequence=120
archived log file name=/orabin01/archive/dbprod_1_121_1031136284.arc thread=1 sequence=121
archived log file name=/orabin01/archive/dbprod_1_122_1031136284.arc thread=1 sequence=122
archived log file name=/orabin01/archive/dbprod_1_123_1031136284.arc thread=1 sequence=123
archived log file name=/orabin01/archive/dbprod_1_124_1031136284.arc thread=1 sequence=124
archived log file name=/orabin01/archive/dbprod_1_125_1031136284.arc thread=1 sequence=125
archived log file name=/orabin01/archive/dbprod_1_126_1031136284.arc thread=1 sequence=126
archived log file name=/orabin01/archive/dbprod_1_127_1031136284.arc thread=1 sequence=127
archived log file name=/orabin01/archive/dbprod_1_128_1031136284.arc thread=1 sequence=128
archived log file name=/orabin01/archive/dbprod_1_129_1031136284.arc thread=1 sequence=129
archived log file name=/orabin01/archive/dbprod_1_130_1031136284.arc thread=1 sequence=130
archived log file name=/orabin01/archive/dbprod_1_131_1031136284.arc thread=1 sequence=131
archived log file name=/orabin01/archive/dbprod_1_132_1031136284.arc thread=1 sequence=132
archived log file name=/orabin01/archive/dbprod_1_133_1031136284.arc thread=1 sequence=133
archived log file name=/orabin01/archive/dbprod_1_134_1031136284.arc thread=1 sequence=134
archived log file name=/orabin01/archive/dbprod_1_135_1031136284.arc thread=1 sequence=135
archived log file name=/orabin01/archive/dbprod_1_136_1031136284.arc thread=1 sequence=136
archived log file name=/orabin01/archive/dbprod_1_137_1031136284.arc thread=1 sequence=137
archived log file name=/orabin01/archive/dbprod_1_138_1031136284.arc thread=1 sequence=138
archived log file name=/orabin01/archive/dbprod_1_139_1031136284.arc thread=1 sequence=139
archived log file name=/orabin01/archive/dbprod_1_140_1031136284.arc thread=1 sequence=140
archived log file name=/orabin01/archive/dbprod_1_141_1031136284.arc thread=1 sequence=141
archived log file name=/orabin01/archive/dbprod_1_142_1031136284.arc thread=1 sequence=142
archived log file name=/orabin01/archive/dbprod_1_143_1031136284.arc thread=1 sequence=143
archived log file name=/orabin01/archive/dbprod_1_144_1031136284.arc thread=1 sequence=144
archived log file name=/orabin01/archive/dbprod_1_145_1031136284.arc thread=1 sequence=145
archived log file name=/orabin01/archive/dbprod_1_146_1031136284.arc thread=1 sequence=146
archived log file name=/orabin01/archive/dbprod_1_147_1031136284.arc thread=1 sequence=147
archived log file name=/orabin01/archive/dbprod_1_148_1031136284.arc thread=1 sequence=148
archived log file name=/orabin01/archive/dbprod_1_149_1031136284.arc thread=1 sequence=149
archived log file name=/orabin01/archive/dbprod_1_150_1031136284.arc thread=1 sequence=150
archived log file name=/orabin01/archive/dbprod_1_151_1031136284.arc thread=1 sequence=151
archived log file name=/orabin01/archive/dbprod_1_152_1031136284.arc thread=1 sequence=152
archived log file name=/orabin01/archive/dbprod_1_153_1031136284.arc thread=1 sequence=153
archived log file name=/orabin01/archive/dbprod_1_154_1031136284.arc thread=1 sequence=154
media recovery complete, elapsed time: 00:00:21
Finished recover at 02-FEB-20

RMAN>
```