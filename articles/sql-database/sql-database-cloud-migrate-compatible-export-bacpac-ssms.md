
<properties
   pageTitle="Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei mit SQL Server Management Studio | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank exportieren, BACPAC-Datei exportieren, Assistent „Datenebenenanwendung exportieren“"
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="05/31/2016"
   ms.author="carlrab"/>

# Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei mit SQL Server Management Studio

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

 
In diesem Artikel wird beschrieben, wie Sie mithilfe des Assistenten „Datenebenenanwendung exportieren“ in SQL Server Management Studio eine SQL Server-Datenbank in eine [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-Datei exportieren.

1. Stellen Sie sicher, dass Sie über die neueste Version von SQL Server Management Studio verfügen. Neue Versionen von Management Studio werden monatlich aktualisiert, damit sie mit Updates des Azure-Portals synchron sind.

	 > [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Öffnen Sie Management Studio, und stellen Sie eine Verbindung mit Ihrer Quelldatenbank im Objekt-Explorer her.

	![Exportieren von Datenebenenanwendungen im Menü "Aufgaben"](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)

3. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Quelldatenbank, zeigen Sie auf **Aufgaben**, und klicken Sie auf **Datenebenenanwendung exportieren…**.

	![Exportieren von Datenebenenanwendungen im Menü "Aufgaben"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Konfigurieren Sie im Export-Assistenten den Export für das Speichern der BACPAC-Datei entweder an einem lokalen Speicherort auf dem Datenträger oder in einem Azure-Blob. Die exportierte BACPAC-Datei enthält immer das vollständige Datenbankschema und standardmäßig die Daten aus allen Tabellen. Verwenden Sie die Registerkarte "Erweitert", wenn Sie Daten aus einigen oder allen Tabellen ausschließen möchten. Sie können z. B. nur die Daten für Verweistabellen exportieren, anstatt die Daten aller Tabellen.

***Wichtig*** Verwenden Sie beim Exportieren einer BACPAC-Datei in Azure-Blobspeicher den Standardspeicher. Das Importieren einer BACPAC-Datei aus dem Premiumspeicher wird nicht unterstützt.

	![Export settings](./media/sql-database-cloud-migrate/MigrateUsingBACPAC02.png)


## Nächste Schritte

- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe von SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe von SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-import.md)
- [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe von PowerShell](sql-database-import-powershell.md)

## Zusätzliche Ressourcen

- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)

<!---HONumber=AcomDC_0615_2016-->