<properties
	pageTitle="Wiederherstellen einer gelöschten Azure SQL-Datenbank (PowerShell) | Microsoft Azure"
	description="Wiederherstellen einer gelöschten Azure SQL-Datenbank (PowerShell)"
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="06/09/2016"
	ms.author="sstein"
	ms.workload="sqldb-bcdr"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Wiederherstellen einer gelöschten Azure SQL-Datenbank mit PowerShell

> [AZURE.SELECTOR]
- [Übersicht](sql-database-recovery-using-backups.md)
- [Wiederherstellen einer gelöschten Datenbank: im Azure-Portal](sql-database-restore-deleted-database-portal.md)

[AZURE.INCLUDE [Starten der PowerShell-Sitzung](../../includes/sql-database-powershell.md)]


## Wiederherstellen der gelöschten Datenbank in einer eigenständigen Datenbank

1. Rufen Sie mit dem Cmdlet [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387.aspx) die Sicherung der gelöschten Datenbank ab, die Sie wiederherstellen möchten.

        $DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Starten Sie mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx) die Wiederherstellung auf der Grundlage der Sicherung der gelöschten Datenbank.
    
        Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"

## Wiederherstellen der gelöschten Datenbank in einem Pool für elastische Datenbanken

1. Rufen Sie mit dem Cmdlet [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387.aspx) die Sicherung der gelöschten Datenbank ab, die Sie wiederherstellen möchten.

        $DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Starten Sie mit dem Cmdlet [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx) die Wiederherstellung auf der Grundlage der Sicherung der gelöschten Datenbank.
    
        Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID –ElasticPoolName "elasticpool01" 

## Nächste Schritte

- Eine Übersicht zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
- Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
- Informationen über Entwurfs- und Wiederherstellungsszenarien für die Geschäftskontinuität finden Sie unter [Geschäftskontinuitätsszenarien](sql-database-business-continuity-scenarios.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
- Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).

<!---HONumber=AcomDC_0629_2016-->