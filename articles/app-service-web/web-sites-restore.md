<properties 
	pageTitle="Wiederherstellen einer App in Azure" 
	description="Erfahren Sie, wie Sie Ihre App aus einer Sicherung wiederherstellen." 
	services="app-service" 
	documentationCenter="" 
	authors="cephalin" 
	manager="wpickett" 
	editor="jimbe"/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/06/2016" 
	ms.author="cephalin"/>

# Wiederherstellen einer App in Azure

In diesem Artikel erfahren Sie, wie Sie eine App in [Azure App Service](../app-service/app-service-value-prop-what-is.md) wiederherstellen, die zuvor gesichert wurde (siehe [Sichern einer App in Azure](web-sites-backup.md)). Sie können Ihre App mit den zugehörigen verknüpften Datenbanken (SQL-Datenbank oder MySQL) bei Bedarf in einem vorherigen Zustand wiederherstellen oder basierend auf der Sicherung der ursprünglichen App eine neue App erstellen. Das Erstellen einer neuen App, die parallel zur letzten Version ausgeführt wird, kann für A/B-Tests nützlich sein.

Das Wiederherstellen aus Sicherungen ist für Apps verfügbar, die in den Tarifen **Standard** und **Premium** ausgeführt werden. Informationen zum zentralen Hochskalieren der App finden Sie unter [Zentrales Hochskalieren einer App in Azure](web-sites-scale.md). Im Tarif **Premium** ist eine größere Anzahl täglicher Sicherungen zulässig ist als im Tarif **Standard**.

<a name="PreviousBackup"></a>
## Wiederherstellen einer App aus einer vorhandenen Sicherung

1. Klicken Sie auf dem Blatt **Einstellungen** Ihrer App im Azure-Portal auf **Sicherungen**, um das Blatt **Sicherungen** anzuzeigen. Klicken Sie dann in der Befehlsleiste auf **Jetzt wiederherstellen**.
	
	!["Jetzt wiederherstellen" auswählen][ChooseRestoreNow]

3. Wählen Sie auf dem Blatt **Wiederherstellen** zuerst die Sicherungsquelle aus.

	![](./media/web-sites-restore/021ChooseSource.png)
	
	Die Option **App-Sicherung** zeigt alle vorhandenen Sicherungen der aktuellen App, aus denen Sie eine auswählen können. Die Option **Speicher** ermöglicht Ihnen in einem in Ihrem Abonnement vorhandenen Azure Storage-Konto und Container das Auswählen einer Sicherungsdatei im ZIP-Format. Wenn Sie versuchen, eine Sicherung einer anderen Anwendung wiederherzustellen, verwenden Sie die Option **Speicher**.

4. Geben Sie anschließend das Ziel für die App-Wiederherstellung unter **Wiederherstellungsziel** an.

	![](./media/web-sites-restore/022ChooseDestination.png)
	
	>[AZURE.WARNING] Wenn Sie **Überschreiben** wählen, werden alle Daten in Ihrer vorhandenen App gelöscht. Bevor Sie auf **OK** klicken, stellen Sie sicher, dass alles genau Ihren Vorstellungen entspricht.
	
	Sie können **Vorhandene App** auswählen, um die App-Sicherung in einer anderen App in derselben Ressourcengruppe wiederherzustellen. Bevor Sie diese Option verwenden, sollten Sie bereits eine andere App in der Ressourcengruppe erstellt haben, deren Datenbankkonfiguration derjenigen entspricht, die in der App-Sicherung definiert ist.
	
5. Klicken Sie auf **OK**.

<a name="StorageAccount"></a>
## Herunterladen oder Löschen einer Sicherung aus einem Speicherkonto
	
1. Wählen Sie auf dem Hauptblatt **Durchsuchen** des Azure-Portals die Option **Speicherkonten** aus.
	
	Eine Liste Ihrer vorhandenen Speicherkonten wird angezeigt.
	
2. Wählen Sie das Speicherkonto aus, das die herunterzuladende oder zu löschende Sicherung enthält.
	
	Das Blatt für das Speicherkonto wird angezeigt.

3. Wählen Sie auf dem Blatt des Speicherkontos den gewünschten Container aus.
	
	![Container anzeigen][ViewContainers]

4. Wählen Sie die Sicherungsdatei aus, die Sie herunterladen oder löschen möchten.

	![ViewContainers](./media/web-sites-restore/03ViewFiles.png)

5. Klicken Sie je nachdem, was Sie tun möchten, auf **Herunterladen** oder **Löschen**.

<a name="OperationLogs"></a>
## Überwachen eines Wiederherstellungsvorgangs
	
1. Um Details über den Erfolg oder Misserfolg des Wiederherstellungsvorgangs für die App anzuzeigen, wechseln Sie im Azure-Portal zum Blatt **Überwachungsprotokolle**.
	
	Auf dem Blatt **Überwachungsprotokolle** werden alle Ihre Vorgänge samt Ebene, Status, Ressourcen und Zeitdetails angezeigt.
	
2. Scrollen Sie nach unten zum gewünschten Wiederherstellungsvorgang, und klicken Sie darauf, um ihn auszuwählen.

Auf dem Blatt „Details“ werden die verfügbaren Informationen im Zusammenhang mit dem Wiederherstellungsvorgang angezeigt.
	
## Nächste Schritte

Sie können App Service-Apps auch mithilfe der REST-API sichern und wiederherstellen (siehe [Verwenden von REST zum Sichern und Wiederherstellen von App Service-Apps](websites-csm-backup.md)).

>[AZURE.NOTE] Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751) sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 

<!---HONumber=AcomDC_0713_2016-->