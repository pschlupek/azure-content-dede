<properties
   pageTitle="Verwendung des QuickBooks-Connectors in Logik-Apps | Microsoft Azure App Service"
   description="Erstellen und Konfigurieren des QuickBooks-Connectors oder einer API-App und Verwenden in einer Logik-App in Azure App Service"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="sameerch"/>


# Erste Schritte mit dem QuickBooks-Connector und das Hinzufügen zur Logik-App
>[AZURE.NOTE] Diese Version des Artikels gilt für die Logik-Apps-Schemaversion 2014-12-01-preview.

Mit dem QuickBooks-Connector können Sie unterschiedliche QuickBooks-Entitäten erstellen und bearbeiten. Die folgende Tabelle enthält die unterstützten Entitäten.

Entitäten|Beschreibung
---|---
Konto|Das Konto ist Komponente eines Kontenplans und Teil eines Sachkontos. Wird verwendet, um einen Gesamtbetrag für einen bestimmten Verwendungszweck aufzuzeichnen.
CreditMemo|CreditMemo ist eine finanzielle Transaktion, die für eine Rückerstattung oder Gutschrift einer Zahlung oder für einen Zahlungsanteil an verkauften Waren oder Dienstleistungen steht.
Kunde|Ein Kunde ist der Konsument der Dienstleistung oder des Produkts, das Ihr Unternehmen anbietet.
Schätzung|Die Schätzung stellt einen Vorschlag einschließlich Preisangebot für Waren oder Dienstleistungen bei einer finanziellen Transaktion zwischen einem Unternehmen und einem Kunden dar.
Rechnung|Eine Rechnung stellt ein Verkaufsformular dar, mit dem der Kunde für ein Produkt oder einen Dienst bezahlt. Weitere Informationen zur Verwendung des Rechnungsdatenmodells finden Sie hier.
Item|Ein Artikel ist etwas, das Ihr Unternehmen kauft, verkauft oder weiterverkauft, wie z. B. Produkte, Versand- und Bearbeitungsgebühren, Rabatte und Mehrwertsteuer (falls zutreffend). Ein Artikel wird als Position in einer Rechnung oder einem Verkaufsformular angezeigt.
SalesReceipt|Diese Entität repräsentiert die Verkaufsbestätigung, die einem Kunden zugewiesen wird.

Logik-Apps können basierend auf einer Vielzahl von Datenquellen ausgelöst werden und Connectors anbieten, um Daten als Teil des Datenflusses abzurufen und zu verarbeiten. Sie können den QuickBooks-Connector dem geschäftlichen Workflow hinzufügen und Daten im Rahmen dieses Workflows in einer Logik-App verarbeiten.

##QuickBooks-Aktionen ##
Im Folgenden werden die verschiedenen im QuickBooks-Connector verfügbaren Aktionen aufgeführt.

Aktion|Beschreibung
---|---
Entität lesen|Liest das Entitätsobjekt.
Entität erstellen oder aktualisieren|Erstellt oder aktualisiert das Entitätsobjekt. Das Objekt wird aktualisiert, wenn es vorhanden ist. Andernfalls wird ein neues Objekt erstellt.
Löschen|Durch diese Aktion wird das angegebene Objekt aus der ausgewählten Entität gelöscht.
Abfragen|Der Abfragevorgang stellt die Methode zum Erstellen einer angeleiteten Abfrage für eine Entität dar.

##Erstellen einer QuickBooks-Connector-API-App##
1.	Öffnen Sie den Azure Marketplace mit der Option "+NEU" unten rechts im Azure-Portal.
2.	Wechseln Sie zu "Web und Mobil > API-Apps", und suchen Sie nach "QuickBooks".
3.	Konfigurieren Sie den QuickBooks-Connector, indem Sie die Informationen für den Hostingplan und die Ressourcengruppe angeben und einen Namen für die API-App wählen.

	![][13]
4. Konfigurieren Sie die QuickBooks-Entitäten, die Sie lesen/schreiben möchten, in den "Paketeinstellungen".

Mit diesen Informationen können Sie jetzt eine QuickBooks-Connector-API-App erstellen.


##Erstellen einer Logik-App##
Wir erstellen eine einfache Logik-App, die ein Konto in QuickBooks erstellt, und aktualisieren den Kategorietyp desselben Kontos.

1.	Melden Sie sich beim Azure-Portal an, und klicken Sie auf „Neu -> Web und mobil -> Logik-App“:

	![][1]

2.	Stellen Sie auf der Seite "Logik-App erstellen" die erforderlichen Details wie Name, App Service-Plan und -Ort bereit.

	![][2]

3.	Klicken Sie auf „Trigger und Aktionen“. Der Bildschirm mit dem Logik-App-Editor wird angezeigt.

	![][3]

4.	Wählen Sie „Diese Logik manuell ausführen“, sodass diese Logik-App nur manuell aufgerufen werden kann:


5.	Erweitern Sie „API-Apps in dieser Ressourcengruppe“ im Katalog, um alle verfügbaren API-Apps anzuzeigen. Wählen Sie „QuickBooks-Connector“ aus dem Katalog aus. Der „QuickBooks-Connector“ wird dem Workflow hinzugefügt.


6.	Wenn Sie QuickBooks online verwenden, müssen Logik-Apps authentifiziert und autorisiert werden, damit sie Vorgänge in Ihrem Namen ausführen können. Zum Starten der Autorisierung klicken Sie im QuickBooks-Connector auf „Autorisieren“.

	![][4]

7.	Durch das Klicken auf "Autorisieren" wird das Authentifizierungsdialogfeld von QuickBook angezeigt. Geben Sie die Anmeldeinformationen des QuickBook-Kontos an, mit dem Sie die Vorgänge ausführen möchten.

	![][5]

8. Gewähren Sie Logik-Apps Zugriff auf Ihr Konto, sodass Vorgänge in Ihrem Namen ausgeführt werden können, indem Sie im Zustimmungsdialogfeld auf „Autorisieren“ klicken.

	![][6]

9.	Sobald die Autorisierung abgeschlossen ist, werden alle Aktionen angezeigt.

	![][7]

10.	Wählen Sie "Konto erstellen oder aktualisieren" aus, sodass die Eingabeparameter angezeigt werden.

	![][8]

11.	Geben Sie "Name" und "Kontotyp" ein, und klicken Sie auf ✓.

	![][9]

12.	Wählen Sie im Katalog im Abschnitt „Zuletzt verwendet“ die Option „QuickBooks-Connector“. Eine neue QuickBooks-Aktion wird hinzugefügt.

13.	Wählen Sie "Konto erstellen oder aktualisieren" aus der Liste der Aktionen aus. Die Eingabeparameter der Aktion werden angezeigt.

	![][10]

14.	Klicken Sie neben ID auf "+", um den "ID"-Wert aus der Ausgabe der Aktion "Konto erstellen" auszuwählen.

	![][11]

15.	Geben Sie neue Werte für den Kontotyp ein, und klicken Sie auf ✓.

	![][12]

16. Klicken Sie im Bildschirm des Logik-App-Editors auf "OK", und klicken Sie dann auf "Erstellen". Es dauert ungefähr 30 Sekunden, bis die Erstellung abgeschlossen ist.

17. Sehen Sie sich die neu erstellte Logik-App an, und klicken Sie zum Initiieren der Ausführung auf „Ausführen“.

18. Sie können überprüfen, ob ein neues Konto mit dem Namen "Contoso" in Ihrem QuickBooks-Konto erstellt wird.

## Mehr mit Ihrem Connector machen
Der Connector ist nun erstellt, und Sie können ihn mit einer Logik-App einem Geschäftsworkflow hinzufügen. Informationen finden Sie unter [Was sind Logik-Apps?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Wenn Sie Azure Logik-Apps ausprobieren möchten, ehe Sie sich für ein Azure-Konto registrieren, wechseln Sie zu [Logik-App testen](https://tryappservice.azure.com/?appservice=logic). Dort können Sie sofort eine kurzlebige Starter-Logik-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.

Anzeigen der Swagger-REST-API-Referenz unter [Referenz zu Connectors und API-Apps](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Sie können auch Leistungsstatistiken überprüfen und die Sicherheit zum Connector steuern. Informationen finden Sie unter [Verwalten und Überwachen integrierter API-Apps und Connectors](app-service-logic-monitor-your-connectors.md).

<!--Image references-->
[1]: ./media/app-service-logic-connector-quickbooks/1_New_Logic_App.png
[2]: ./media/app-service-logic-connector-quickbooks/2_Logic_App_Settings.png
[3]: ./media/app-service-logic-connector-quickbooks/3_Logic_App_Editor.png
[4]: ./media/app-service-logic-connector-quickbooks/4_QuickBooks_Authorize.png
[5]: ./media/app-service-logic-connector-quickbooks/5_QuickBooks_Login.png
[6]: ./media/app-service-logic-connector-quickbooks/6_QuickBooks_User_Consent.png
[7]: ./media/app-service-logic-connector-quickbooks/7_QuickBooks_Actions.png
[8]: ./media/app-service-logic-connector-quickbooks/8_QuickBooks_Create_Account.png
[9]: ./media/app-service-logic-connector-quickbooks/9_Create_Account_OK.png
[10]: ./media/app-service-logic-connector-quickbooks/10_QuickBooks_Update_Account.png
[11]: ./media/app-service-logic-connector-quickbooks/11_Record_ID_from_Create.png
[12]: ./media/app-service-logic-connector-quickbooks/12_Update_Account_Address.png
[13]: ./media/app-service-logic-connector-quickbooks/13_Create_new_quickbooks_connector.png

<!---HONumber=AcomDC_0601_2016-->