<properties
    pageTitle="Hinzufügen des Dropbox-Connectors zu PowerApps Enterprise oder Logik-Apps | Microsoft Azure"
    description="Übersicht über den Dropbox-Connector mit REST-API-Parametern"
    services=""
    suite=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="05/20/2016"
   ms.author="mandia"/>

# Erste Schritte mit dem Dropbox-Connector 
Verbinden Sie sich mit Dropbox, um Dateien zu verwalten, d. h. erstellen, abrufen usw. Der Dropbox-Connector kann verwendet werden in:

- Logik-Apps 
- PowerApps

> [AZURE.SELECTOR]
- [Logik-Apps](../articles/connectors/connectors-create-api-dropbox.md)
- [PowerApps Enterprise](../articles/power-apps/powerapps-create-api-dropbox.md)

&nbsp;

>[AZURE.NOTE] Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.


Dropbox ermöglicht Folgendes:

- Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Dropbox abgerufen werden. 
- Verwenden von Triggern, wenn eine Datei erstellt oder aktualisiert wird.
- Verwenden von Aktionen, um z. B. eine Datei zu kopieren oder zu löschen. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Wenn eine neue Datei in Dropbox erstellt wird, können Sie die Datei mit Office 365 per E-Mail senden.
- Fügen Sie den Dropbox-Connector zu PowerApps Enterprise hinzu. Die Benutzer können diesen Connector anschließend in ihren Apps verwenden. 

Informationen zum Hinzufügen eines Connectors in PowerApps Enterprise finden Sie unter [Registrieren einer Microsoft-verwalteten API oder einer IT-verwalteten API](../power-apps/powerapps-register-from-available-apis.md).

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Trigger und Aktionen
Dropbox weist die folgenden Trigger und Aktionen auf.

Trigger | Aktionen
--- | ---
<ul><li>Wenn eine Datei erstellt wird</li><li>Wenn eine Datei geändert wird</li></ul> | <ul><li>Datei erstellen</li><li>Wenn eine Datei erstellt wird</li><li>Datei kopieren</li><li>Datei löschen</li><li>Archiv in Ordner extrahieren</li><li>Dateiinhalt anhand der ID abrufen</li><li>Datei mithilfe des Pfads abrufen</li><li>Dateimetadaten anhand der ID abrufen</li><li>Dateimetadaten anhand des Pfads abrufen</li><li>Datei aktualisieren</li><li>Wenn eine Datei geändert wird</li></ul>

Alle Connectors unterstützen Daten im JSON- und XML-Format.

## Herstellen der Verbindung mit Dropbox

Wenn Sie diesen Connector Ihren Logik-Apps hinzufügen, müssen Sie den Logik-Apps das Herstellen einer Verbindung mit Ihrer Dropbox erlauben.

>[AZURE.INCLUDE [Schritte zum Herstellen einer Verbindung mit Dropbox](../../includes/connectors-create-api-dropbox.md)]

Nachdem Sie eine Verbindung hergestellt haben, geben Sie die Dropbox-Eigenschaften ein, z. B. Ordnerpfad oder Dateiname. In der **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

>[AZURE.TIP] Sie können dieselbe Dropbox-Verbindung in anderen Logik-Apps verwenden.

## Swagger-REST-API – Referenz
Gilt für Version: 1.0.

### Datei erstellen    
Lädt eine Datei in Dropbox hoch. ```POST: /datasets/default/files```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|folderPath|string|Ja|query|(Keine) |Ordnerpfad zum Hochladen der Datei in Dropbox|
|name|string|Ja|query|(Keine) |Name der Datei, die in Dropbox erstellt werden soll|
|body|string(binary) |Ja|body|(Keine) |Inhalt der Datei, die in Dropbox hochgeladen werden soll|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Wenn eine Datei erstellt wird    
Löst einen Datenfluss aus, wenn in einem Dropbox-Ordner eine neue Datei erstellt wird. ```GET: /datasets/default/triggers/onnewfile```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|folderId|string|Ja|query|(Keine) |Eindeutiger Bezeichner des Ordners in Dropbox|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Datei kopieren    
Kopiert eine Datei in Dropbox. ```POST: /datasets/default/copyFile```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|source|string|Ja|query|(Keine) |URL zur Quelldatei|
|destination|string|Ja|query| (Keine)|Zieldateipfad in Dropbox, einschließlich Zieldateiname|
|overwrite|Boolescher Wert|no|query|(Keine) |Überschreibt die Zieldatei, falls auf „True“ festgelegt|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Datei löschen    
Löscht eine Datei aus Dropbox. ```DELETE: /datasets/default/files/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner der aus Dropbox zu löschenden Datei|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Archiv in Ordner extrahieren    
Extrahiert eine Archivdatei in einen Ordner in Dropbox (Beispiel: .zip). **```POST: /datasets/default/extractFolderV2```**

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|source|string|Ja|query|(Keine) |Pfad zur Archivdatei|
|destination|string|Ja|query|(Keine) |Pfad in Dropbox, in den der Archivinhalt extrahiert wird|
|overwrite|Boolescher Wert|no|query|(Keine) |Überschreibt die Zieldateien, falls auf „True“ festgelegt|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Dateiinhalt anhand der ID abrufen    
Ruft Dateiinhalte mithilfe der ID aus Dropbox ab. ```GET: /datasets/default/files/{id}/content```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|id|string|Ja|path|(Keine) |Eindeutiger Bezeichner der Datei in Dropbox|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Dateiinhalt anhand des Pfads abrufen    
Ruft Dateiinhalte mithilfe des Pfads aus Dropbox ab. ```GET: /datasets/default/GetFileContentByPath```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|path|string|Ja|query|(Keine) |Eindeutiger Pfad zur Datei in Dropbox|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Dateimetadaten anhand der ID abrufen    
Ruft Dateimetadaten mithilfe der Datei-ID aus Dropbox ab. ```GET: /datasets/default/files/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|id|string|Ja|path|(Keine) |Eindeutiger Bezeichner der Datei in Dropbox|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Dateimetadaten anhand des Pfads abrufen    
Ruft Dateimetadaten mithilfe des Pfads aus Dropbox ab. ```GET: /datasets/default/GetFileByPath```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|path|string|Ja|query|(Keine) |Eindeutiger Pfad zur Datei in Dropbox|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Datei aktualisieren    
Aktualisiert eine Datei in Dropbox. ```PUT: /datasets/default/files/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|id|string|Ja|path| (Keine)|Eindeutiger Bezeichner der Datei, die in Dropbox aktualisiert werden soll|
|body|string(binary) |Ja|body|(Keine) |Inhalt der Datei, die in Dropbox aktualisiert werden soll|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Wenn eine Datei geändert wird    
Löst einen Datenfluss aus, wenn in einem Dropbox-Ordner eine Datei geändert wird. ```GET: /datasets/default/triggers/onupdatedfile```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|folderId|string|Ja|query|(Keine) |Eindeutiger Bezeichner des Ordners in Dropbox|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## Objektdefinitionen

#### DataSetsMetadata

|Eigenschaftenname | Datentyp | Erforderlich|
|---|---|---|
|tabular|nicht definiert|no|
|Blob|nicht definiert|no|

#### TabularDataSetsMetadata

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|source|string|no|
|displayName|string|no|
|urlEncoding|string|no|
|tableDisplayName|string|no|
|tablePluralName|string|no|

#### BlobDataSetsMetadata

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|source|string|no|
|displayName|string|no|
|urlEncoding|string|no|

#### BlobMetadata

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|ID|string|no|
|Name|string|no|
|DisplayName|string|no|
|Path|string|no|
|LastModified|string|no|
|Größe|integer|no|
|MediaType|string|no|
|IsFolder|Boolescher Wert|no|
|ETag|string|no|
|FileLocator|string|no|

## Nächste Schritte

[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

Gehen Sie zur [Liste der APIs](apis-list.md) zurück.


<!--References-->
[1]: https://www.dropbox.com/login
[2]: https://www.dropbox.com/developers/apps/create
[3]: https://www.dropbox.com/developers/apps
[8]: ./media/connectors-create-api-dropbox/dropbox-developer-site.png
[9]: ./media/connectors-create-api-dropbox/dropbox-create-app.png
[10]: ./media/connectors-create-api-dropbox/dropbox-create-app-page1.png
[11]: ./media/connectors-create-api-dropbox/dropbox-create-app-page2.png

<!---HONumber=AcomDC_0525_2016-->