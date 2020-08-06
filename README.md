# Zabbix template for Microsoft SQL Server Backup
Zabbix template for Microsoft SQL Server (MS SQL) backup status.


### Features
+ MS SQL instance Low Level Discovery.
+ MS SQL database Low Level Discovery.
+ MS SQL database backup status Low Level Discovery.

### Supported versions
+ Microsoft SQL Server 2012, 2014 and 2016
+ Zabbix 5.0.2.

## Template:
+ "dotup-zabbix-template-mssql-backup.xml" – Template for Microsoft SQL Server database backup monitoring.

## Config:
+ "dotup-zabbix-template-mssql-backup.userparams.conf" – user parameters.

## Scripts:
+ "Discovery.mssql.sysdatabasefull.ps1" - PowerShell script for Low Level Discovery.
+ "Discovery.mssql.sysdatabasename.ps1" - PowerShell script for Low Level Discovery.
+ "Discovery.mssql.userdatabasediff.ps1" - PowerShell script for Low Level Discovery.
+ "Discovery.mssql.userdatabasefull.ps1" - PowerShell script for Low Level Discovery.
+ "Discovery.mssql.userdatabaselog.ps1" - PowerShell script for Low Level Discovery.
+ "Discovery.mssql.userdatabasename.ps1" - PowerShell script for Low Level Discovery.

## Deployment Step by step
1. Import templates via Configuration >> Templates:
2. "dotup-zabbix-template-mssql-backup.xml"
3. Copy PowerShell scripts (*.ps1) to "C:\Program Files\Zabbix Agent\dotup\DiscoveryDatabaseBackup\".
4. Copy dotup-zabbix-template-mssql-backup.userparams.conf to "C:\Program Files\Zabbix Agent\dotup\DiscoveryDatabaseBackup".
5. Modify file "zabbix_agentd.conf" - add line "Include=C:\Program Files\Zabbix Agent\dotup\DiscoveryDatabaseBackup\dotup-zabbix-template-mssql-backup.userparams.conf".
6. Grant rights for Zabbix Agent service account. It needs read rights on tables master.sys.databases and
msdb.dbo.backupset. By default, Zabbix Agent service account is NT AUTHORITY\SYSTEM which is
already in SQL Server.
7. Restart Zabbix Agent.
8. Modify macros in the template according to your needs.
9. Add template to a Host.

## Macros Macros meaning Value Value meaning Trigger

+ {$SYSDBFTIME1} System db full backup time value 1 - 25 - 25 hours - Low
+ {$SYSDBFTIME2} System db full backup time value 2 - 26 - 26 hours - Medium
+ {$SYSDBFTIME3} System db full backup time value 3 - 27 - 27 hours - High

+ {$UDBDTIME1} User db diff backup time value 1 - 25 - 25 hours - Low
+ {$UDBDTIME2} User db diff backup time value 2 - 26 - 26 hours - Medium
+ {$UDBDTIME3} User db diff backup time value 3 - 27 - 27 hours - High

+ {$UDBFTIME1} User db full backup time value 1 - 172 - 7 days and 4 hours - Low
+ {$UDBFTIME2} User db full backup time value 2 - 174 - 7 days and 6 hours - Medium
+ {$UDBFTIME3} User db full backup time value 3 - 176 - 7 days and 8 hours - High

+ {$UDBLTIME1} User db log backup time value 1 - 30 - 30 minutes - Low
+ {$UDBLTIME2} User db log backup time value 2 - 60 - 60 minutes - Medium
+ {$UDBLTIME3} User db log backup time value 3 - 90 - 90 minutes - High

## Interval
Default Items Update interval (5m seconds), History storage period (60 days) and Trend storage period (90 days)

## Applications
+ Database
+ MSSQL
+ MSSQL - {#SQLINSTANCENAME}
+ MSSQL - {#SQLINSTANCENAME} - System Database {#DBNAME}
+ MSSQL - {#SQLINSTANCENAME} - Database {#DBNAME}

# Notes

1. MS SQL backup history is queried from table msdb.dbo.backupset. If there is no info in this table or the info is incorrect, template may not work correctly. I know there are 3rd party backup solutions, which may insert incorrect values or do not insert at all. If you want only a specific host to have other values than default, you can change them in that host’s macros section. Zabbix value mapping section does not support macros, therefore, if you change a default macros values for a template or a specific host, remember to update "SQL Database Backup status" via Administration >> General >> Value mapping.

2. If you have a non-default Zabbix installation directory or you want to put Powershell scripts to other directory, you will have to update files and directories in steps 3- 7. Userparamaters moved to a separate file "mssql.backup.userparams.conf" for an easier management in a present and a future. All Powershell scripts moved to a separate directory "C:\Program Files\Zabbix Agent\dotup\DiscoveryDatabaseBackup\" for an easier management in a present and a future.

## Thanks
Based on https://github.com/MantasTumenas/Zabbix-template-for-Microsoft-SQL-Server-Backup

# dotup IT solutions
+ https://dotup.de
+ https://blog.dotup.de
