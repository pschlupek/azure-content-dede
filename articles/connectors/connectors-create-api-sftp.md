<properties
    pageTitle="Hinzufügen des SFTP-Connectors zu Ihren Logik-Apps | Microsoft Azure"
    description="Übersicht über den SFTP-Connector mit REST-API-Parametern"
    services=""
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
   ms.date="05/18/2016"
   ms.author="mandia"/>

# Erste Schritte mit dem SFTP-Connector 
Sie stellen eine Verbindung mit einem SFTP-Server her, um Dateien zu verwalten. Sie können auf dem SFTP-Server verschiedene Aufgaben ausführen, z. B. Dateien hochladen, Dateien löschen und mehr. Der SFTP-Connector kann verwendet werden in:

- Logik-Apps

>[AZURE.NOTE] Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.

Mit SFTP können Sie folgende Aktionen ausführen:

- Erstellen eines Geschäftsworkflows basierend auf den Daten, die über SFTP abgerufen werden. 
- Implementieren eines Triggers, wenn eine Datei aktualisiert wird.
- Ausführen von Aktionen, um Dateien zu erstellen, zu löschen usw. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Sie können z. B. den Inhalt einer Datei abrufen und dann eine SQL-Datenbank aktualisieren. 

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).


## Trigger und Aktionen
Der SFTP-Connector verfügt über folgende Trigger und Aktionen.

Trigger | Actions
--- | ---
<ul><li>Wenn eine Datei geändert oder erstellt wird </li></ul> | <ul><li>Datei erstellen</li><li>Datei kopieren</li><li>Datei löschen</li><li>Ordner extrahieren</li><li>Dateiinhalt abrufen</li><li>Dateiinhalt anhand des Pfads abrufen</li><li>Dateimetadaten abrufen</li><li>Dateimetadaten anhand des Pfads abrufen</li><li>Datei aktualisieren</li><li>Wenn eine Datei erstellt oder geändert wird </li></ul>

Alle Connectors unterstützen Daten im JSON- und XML-Format.


## Herstellen einer Verbindung mit SFTP
Wenn Sie diesen Connector Ihren Logik-Apps hinzufügen, geben Sie die folgenden Werte ein:

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Host Server Address| Ja | Geben Sie den vollqualifizierten Domänennamen (FQDN) oder die IP-Adresse des SFTP-Servers ein.|
|Benutzername| Ja | Geben Sie den Benutzernamen für die Verbindung mit dem SFTP-Server ein.|
|Kennwort | Ja | Geben Sie das Kennwort für den Benutzernamen ein.|
|SSH Server Host Key Finger Print | Ja | Geben Sie den Fingerabdruck des öffentlichen Hostschlüssels für den SSH-Server ein. <br/><br/>In der Regel erhalten Sie diesen Schlüssel vom Serveradministrator. Sie können auch die Tools ```WinSCP``` oder ```ssh-keygen-g3 -F``` verwenden, um den Hostschlüssel-Fingerabdruck abzurufen. | 

Hier ist eine schrittweise Vorgehensweise zum Herstellen der Verbindung:

>[AZURE.INCLUDE [Schritte zum Herstellen einer Verbindung mit SFTP](../../includes/connectors-create-api-sftp.md)]

Nachdem Sie eine Verbindung hergestellt haben, geben Sie die SFTP-Eigenschaften ein, z. B. Ordnerpfad oder Datei. In der **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

>[AZURE.TIP] Sie können dieselbe SFTP-Verbindung in anderen Logik-Apps verwenden.


## Swagger-REST-API – Referenz
Gilt für Version: 1.0.

### Datei erstellen
Lädt eine Datei in SFTP hoch. ```POST: /datasets/default/files```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|folderPath|string|Ja|query|(Keine) |Eindeutiger SFTP-Ordnerpfad|
|name|string|Ja|query| (Keine)|Name der Datei|
|body|string(binary) |Ja|body|(Keine) |Inhalt der Datei, die auf dem SFTP-Server erstellt werden soll|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|

### Datei kopieren
Kopiert eine Datei in SFTP. ```POST: /datasets/default/copyFile```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|source|string|Ja|query| (Keine)|Pfad zur Quelldatei|
|Ziel|string|Ja|query|(Keine) |Pfad zur Zieldatei, einschließlich des Dateinamens|
|overwrite|Boolescher Wert|no|query|(Keine)|Überschreibt die Zieldatei, falls auf „True“ festgelegt|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|

### Datei löschen 
Löscht eine Datei in SFTP. ```DELETE: /datasets/default/files/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|id|string|Ja|path|(Keine) |Eindeutiger Bezeichner der Datei auf dem SFTP-Server|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|

### Ordner extrahieren
Extrahiert eine Archivdatei in einen Ordner mithilfe von SFTP (Beispiel: .zip). ```POST: /datasets/default/extractFolderV2```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|source|string|Ja|query|(Keine) |Pfad zur Archivdatei|
|destination|string|Ja|query|(Keine) |Pfad zum Zielordner|
|overwrite|Boolescher Wert|no|query|(Keine)|Überschreibt die Zieldateien, falls auf „True“ festgelegt|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|

### Dateiinhalte abrufen
Ruft Dateiinhalte mithilfe der ID von SFTP ab. ```GET: /datasets/default/files/{id}/content```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|id|string|Ja|path|(Keine) |Eindeutiger Bezeichner der Datei auf dem SFTP-Server|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Dateiinhalt anhand des Pfads abrufen
Ruft Dateiinhalte mithilfe des Pfads von SFTP ab. ```GET: /datasets/default/GetFileContentByPath```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|path|string|Ja|query| (Keine)|Eindeutiger Pfad zur Datei auf dem SFTP-Server|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Dateimetadaten abrufen 
Ruft Dateimetadaten von SFTP mithilfe der Datei-ID ab. ```GET: /datasets/default/files/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|id|string|Ja|path| (Keine)|Eindeutiger Bezeichner der Datei auf dem SFTP-Server|

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 | OK | 
| default | Fehler beim Vorgang.


### Dateimetadaten anhand des Pfads abrufen
Ruft Dateimetadaten von SFTP mithilfe des Pfads ab. ```GET: /datasets/default/GetFileByPath```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|path|string|Ja|query|(Keine) |Eindeutiger Pfad zur Datei auf dem SFTP-Server|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Datei aktualisieren
Aktualisiert den Dateiinhalt mithilfe von SFTP. ```PUT: /datasets/default/files/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|id|string|Ja|path|(Keine) |Eindeutiger Bezeichner der Datei auf dem SFTP-Server|
|body|string(binary) |Ja|body| (Keine)|Inhalt der Datei, die auf dem SFTP-Server aktualisiert werden soll|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Wenn eine Datei oder geändert erstellt wird 
Löst einen Workflow aus, wenn eine Datei in SFTP geändert wird. ```GET: /datasets/default/triggers/onupdatedfile```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|folderId|string|Ja|query|(Keine) |Eindeutiger Bezeichner des Ordners|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## Objektdefinitionen

#### DataSetsMetadata

| Name | Datentyp | Erforderlich|
|---|---|---|
|tabular|nicht definiert|no|
|Blob|nicht definiert|no|

#### TabularDataSetsMetadata

| Name | Datentyp | Erforderlich|
|---|---|---|
|source|string|no|
|displayName|string|no|
|urlEncoding|string|no|
|tableDisplayName|string|no|
|tablePluralName|string|no|

#### BlobDataSetsMetadata

| Name | Datentyp | Erforderlich|
|---|---|---|
|source|string|no|
|displayName|string|no|
|urlEncoding|string|no|

#### BlobMetadata

| Name | Datentyp | Erforderlich|
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

<!---HONumber=AcomDC_0525_2016-->