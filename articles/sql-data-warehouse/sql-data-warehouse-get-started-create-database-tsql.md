<properties
   pageTitle="Erstellen eines SQL Data Warehouse mit TSQL | Microsoft Azure"
   description="Erfahren Sie, wie Sie ein Azure SQL Data Warehouse mit TSQL erstellen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/20/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Erstellen einer SQL Data Warehouse-Datenbank mithilfe von Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Azure-Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

In diesem Artikel erfahren Sie, wie Sie eine SQL Data Warehouse-Instanz mithilfe von T-SQL erstellen.

## Voraussetzungen

Zunächst benötigen Sie Folgendes:

- **Azure-Konto:** Lesen Sie zum Erstellen eines Kontos die Informationen unter [Kostenlose Azure-Testversion][] oder [MSDN-Azure-Gutschriften][].
- **Azure SQL Server:** Ausführliche Informationen finden Sie unter [Erstellen eines logischen Azure SQL-Datenbankservers mit dem Azure-Portal][] oder [Erstellen einer neuen SQL-Datenbank und Ausführen gängiger Datenbankeinrichtungsaufgaben mithilfe von PowerShell-Cmdlets][].
- **Ressourcengruppe:** Verwenden Sie entweder die gleiche Ressourcengruppe wie Ihre Azure SQL Server-Instanz, oder lesen Sie die Informationen zum [Erstellen einer neuen Ressourcengruppen][].
- **Umgebung für die Ausführung von T-SQL**: Für die Ausführung von T-SQL können Sie [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][] oder [SSMS][] verwenden.

> [AZURE.NOTE] Wenn Sie ein neues SQL Data Warehouse erstellen, wird dadurch unter Umständen auch ein neuer abrechenbarer Dienst erstellt. Unter [SQL Data Warehouse – Preise][] finden Sie weitere Informationen zu den Preisen.

## Erstellen einer Datenbank mit Visual Studio

Wenn Sie mit Visual Studio noch nicht vertraut sind, finden Sie im Artikel [Herstellen einer Verbindung mit SQL Data Warehouse über Visual Studio][] weitere Informationen. Zum Starten öffnen Sie den SQL Server-Objekt-Explorer in Visual Studio und stellen eine Verbindung mit dem Server her, der Ihre SQL Data Warehouse-Datenbank hosten soll. Wenn die Verbindung hergestellt wurde, können Sie eine SQL Data Warehouse-Instanz erstellen, indem Sie den folgenden SQL-Befehl auf die Datenbank **master** anwenden. Dieser Befehl erstellt die Datenbank „MySqlDwDb“ mit dem Dienstziel DW400, und die Datenbank kann maximal 10 TB groß sein.

```sql
CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## Erstellen Sie eine Datenbank mit SQLCMD

Alternativ können Sie den gleichen Befehl mit „sqlcmd“ ausführen, wenn Sie bei einer Eingabeaufforderung Folgendes ausführen.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

`MAXSIZE` kann zwischen 250 GB und 240 TB liegen. `SERVICE_OBJECTIVE` kann einen [DWU][]-Wert zwischen DW100 und DW2000 aufweisen. Eine Liste aller gültigen Werte finden Sie in der MSDN-Dokumentation zu [CREATE DATABASE][]. Sowohl MAXSIZE als auch SERVICE\_OBJECTIVE können auch mit einem [ALTER DATABASE][]-T-SQL-Befehl geändert werden. Seien Sie beim Ändern von SERVICE-OBJECTIVE vorsichtig, da dies einen Neustart des Diensts zur Folge hat, was alle aktuellen Abfragen abbricht. Da MAXSIZE nur ein einfacher Metadatenvorgang ist, werden durch die Änderung dieses Werts keine Dienste neu gestartet.

## Nächste Schritte

Nach der SQL Data Warehouse-Bereitstellung können Sie [Beispieldaten laden][] oder die Schritte zum [Entwickeln][], [Laden][] oder [Migrieren][] lernen.

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: ./sql-data-warehouse-get-started-provision.md
[Herstellen einer Verbindung mit SQL Data Warehouse über Visual Studio]: ./sql-data-warehouse-get-started-connect.md
[Migrieren]: ./sql-data-warehouse-overview-migrate.md
[Entwickeln]: ./sql-data-warehouse-overview-develop.md
[Laden]: ./sql-data-warehouse-overview-load.md
[Beispieldaten laden]: ./sql-data-warehouse-get-started-load-sample-databases.md
[Erstellen eines logischen Azure SQL-Datenbankservers mit dem Azure-Portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Erstellen einer neuen SQL-Datenbank und Ausführen gängiger Datenbankeinrichtungsaufgaben mithilfe von PowerShell-Cmdlets]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Erstellen einer neuen Ressourcengruppen]: ../resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: ./sql-data-warehouse-install-visual-studio.md
[sqlcmd]: ./sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse – Preise]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Kostenlose Azure-Testversion]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN-Azure-Gutschriften]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

<!---HONumber=AcomDC_0727_2016-->