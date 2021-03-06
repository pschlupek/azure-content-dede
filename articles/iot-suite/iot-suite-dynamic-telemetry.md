<properties
	pageTitle="Verwenden dynamischer Telemetriedaten | Microsoft Azure"
	description="In diesem Tutorial erfahren Sie, wie Sie dynamische Telemetriedaten mit der vorkonfigurierten Lösung für die Remoteüberwachung verwenden."
	services=""
    suite="iot-suite"
	documentationCenter=""
	authors="dominicbetts"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/07/2016"
     ms.author="dobett"/>

# Verwenden dynamischer Telemetriedaten mit der vorkonfigurierten Lösung für die Remoteüberwachung

## Einführung

Die an die vorkonfigurierte Lösung für die Remoteüberwachung gesendeten dynamischer Telemetriedaten lassen sich visualisieren. Die simulierten Geräte, die mit der vorkonfigurierten Lösung bereitgestellt werden, senden Telemetriedaten zu Temperatur und Feuchtigkeit, die Sie im Dashboard visualisieren können. Wenn Sie die vorhandenen simulierten Geräte anpassen, neue simulierte Geräte erstellen oder physische Geräte mit der vorkonfigurierten Lösung verbinden, können Sie andere Telemetriewerte wie Außentemperatur, U/min. oder Windgeschwindigkeit senden. Sie können diese zusätzlichen Telemetriedaten dann im Dashboard visualisieren.

Dieses Tutorial verwendet ein einfaches simuliertes Node.js-Gerät, das Sie problemlos anpassen können, um mit dynamischen Telemetriedaten zu experimentieren.

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- Ein aktives Azure-Abonnement. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk_free_trial].
- Mindestens die [Node.js][lnk-node]-Version 0.12.x.

Sie können dieses Tutorial unter allen Betriebssystemen, z.B. Windows oder Linux, durcharbeiten, die die Installation von Node.js unterstützen.

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## Konfigurieren des simulierten Node.js-Geräts

1. Klicken Sie im Dashboard für die Remoteüberwachung auf **+ Gerät hinzufügen**, und fügen Sie ein neues benutzerdefiniertes Gerät hinzu. Notieren Sie sich den IoT Hub-Hostnamen, die Geräte-ID und den Geräteschlüssel. Sie benötigen diese Informationen im weiteren Verlauf dieses Tutorials, wenn Sie die Geräteclientanwendung „remote\_monitoring.js“ vorbereiten.

2. Stellen Sie sicher, dass auf dem Entwicklungscomputer mindestens die Node.js-Version 0.12.x installiert ist. Führen Sie an einer Eingabeaufforderung oder in der Shell `node --version` aus, um die Version zu überprüfen. Informationen zur Verwendung eines Paket-Managers zum Installieren von Node.js unter Linux finden Sie unter [Installieren von Node.js mithilfe eines Paket-Managers][node-linux].

3. Wenn Sie Node.js installiert haben, klonen Sie die neueste Version des Repositorys [azure-iot-sdks][lnk-github-repo] auf Ihren Entwicklungscomputer. Verwenden Sie stets die Verzweigung **master**, die die neueste Version der Bibliotheken und Beispiele bietet.

4. Kopieren Sie in Ihrer lokalen Kopie des Repositorys [azure-iot-sdks][lnk-github-repo] die folgenden beiden Dateien aus dem Ordner „node/device/samples“ in einen leeren Ordner auf Ihrem Entwicklungscomputer:

  - packages.json
  - remote\_monitoring.js

5. Öffnen Sie die Datei „remote\_monitoring.js“, und suchen Sie nach der folgenden Variablendefinition:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

6. Ersetzen Sie **[IoT Hub device connection string]** durch Ihre Geräteverbindungszeichenfolge. Verwenden Sie die Werte für Ihren IoT Hub-Hostnamen, die Geräte-ID und den Geräteschlüssel, die Sie in Schritt 1 notiert haben. Eine Geräteverbindungszeichenfolge hat folgendes Format:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Wenn der IoT Hub-Hostname **contoso** und die Geräte-ID **mydevice** lautet, ergibt sich folgende Verbindungszeichenfolge:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

7. Speichern Sie die Datei. Führen Sie die folgenden Befehle in einer Shell oder an einer Eingabeaufforderung im Ordner mit diesen Dateien aus, um die erforderlichen Pakete zu installieren. Führen Sie dann die Beispielanwendung aus:

    ```
    npm install
    node remote_monitoring.js
    ```

## Beobachten dynamischer Telemetriedaten in Aktion

Das Dashboard zeigt die Telemetriedaten zu Temperatur und Feuchtigkeit von den vorhandenen simulierten Geräten:

![Das Standarddashboard][image1]

Wenn Sie das simulierte Node.js-Gerät auswählen, das Sie im vorherigen Abschnitt ausgeführt haben, werden Telemetriedaten zu Temperatur, Luftfeuchtigkeit und Außentemperatur angezeigt:

![Hinzufügen von Außentemperaturen zum Dashboard][image2]

Die Remoteüberwachungslösung erkennt automatisch die zusätzlichen Telemetriedaten zur Außentemperatur und fügt sie dem Diagramm im Dashboard hinzu.

## Hinzufügen eines neuen Telemetriedatentyps

Der nächste Schritt besteht im Ersetzen der vom simulierten Node.js-Gerät generierten Telemetriedaten durch eine neue Menge von Werten:

1. Beenden Sie das simulierte Node.js-Gerät durch Eingabe von **STRG+C** an der Eingabeaufforderung oder in der Shell.

2. In der Datei „remote\_monitoring.js“ sehen Sie die Basisdatenwerte für die vorhandenen Telemetriedaten für Temperatur, Luftfeuchtigkeit und Außentemperatur. Fügen Sie für **rpm** (U/min.) einen neuen Basisdatenwert hinzu:

    ```
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Das simulierte Node.js-Gerät generiert Telemetriedaten durch Hinzufügen einer zufälligen Erhöhung zu den Stammdatenwerten mithilfe der **generateRandomIncrement**-Funktion in der Datei „remote\_monitoring.js“. Randomisieren Sie den Wert **rpm**, indem Sie hinter der vorhandenen Randomisierung eine Codezeile hinzufügen:

    ```
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Fügen Sie den neuen RPM-Wert der JSON-Nutzlast hinzu, die das Gerät an IoT Hub sendet:

    ```
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Führen Sie das simulierte Node.js-Gerät mit dem folgenden Befehl aus:

    ```
    node remote_monitoring.js
    ```

6. Beobachten Sie den neuen Telemetrietyp „RPM“, der im Diagramm im Dashboard angezeigt wird:

![Hinzufügen von RPM zum Dashboard][image3]

> [AZURE.NOTE] Sie müssen möglicherweise das Node.js-Gerät auf der Seite **Geräte** im Dashboard deaktivieren und aktivieren, damit die Änderung sofort wirksam wird.

## Anpassen der Dashboardanzeige

Die Nachricht **Device-Info** kann Metadaten zur Telemetrie enthalten, die das Gerät an IoT Hub senden kann. Diese Metadaten können die Telemetriedaten angeben, die das Gerät sendet. Ändern Sie den Wert **deviceMetaData** in der Datei „remote\_monitoring.js“ so, dass eine Definition von **Telemetry** auf die Definition von **Commands** folgt, was im folgenden Codeausschnitt gezeigt wird (setzen Sie unbedingt ein `,` hinter die Definition von **Commands**):

```
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [AZURE.NOTE] Die Remoteüberwachungslösung unterscheidet beim Vergleichen der Metadatendefinition mit Daten im Telemetriedatenstrom nicht zwischen Groß-/Kleinschreibung.

Durch Hinzufügen der Definition **Telemetry**, wie im obigen Beispiel, ändert sich nicht das Verhalten des Dashboards. Allerdings können die Metadaten auch das **DisplayName**-Attribut enthalten, um die Anzeige im Dashboard anzupassen. Aktualisieren Sie die Metadatendefinition von **Telemetry** wie folgt:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Der folgende Screenshot zeigt, wie sich durch diese Änderung am Dashboard die Diagrammlegende ändert:

![Anpassen der Diagrammlegende][image4]

> [AZURE.NOTE] Sie müssen möglicherweise das Node.js-Gerät auf der Seite **Geräte** im Dashboard deaktivieren und aktivieren, damit die Änderung sofort wirksam wird.

## Filtern der Telemetrietypen

Standardmäßig zeigt das Diagramm im Dashboard jede Datenreihe im Telemetriedatenstrom an. Mithilfe der Metadaten zu **Device-Info** können Sie die Anzeige bestimmter Telemetrietypen im Diagramm unterdrücken.

Damit im Diagramm nur die Telemetriedaten zu Temperatur und Feuchtigkeit angezeigt werden, entfernen Sie **ExternalTemperature** wie folgt aus den Metadaten zu **Device-Info** **Telemetry**:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

**Outdoor Temperature** wird nicht mehr im Diagramm angezeigt:

![Filtern der Telemetriedaten im Dashboard][image5]

Beachten Sie, dass dies nur die Darstellung des Diagramms betrifft. Die Datenwerte von **ExternalTemperature** werden weiterhin gespeichert und für die Back-End-Verarbeitung zur Verfügung gestellt.

> [AZURE.NOTE] Sie müssen möglicherweise das Node.js-Gerät auf der Seite **Geräte** im Dashboard deaktivieren und aktivieren, damit die Änderung sofort wirksam wird.

## Fehlerbehandlung

Um einen Datenstrom im Diagramm anzuzeigen, muss dessen **Typ** in den Metadaten zu **Device-Info** mit dem Datentyp der Telemetriewerte übereinstimmen. Wenn beispielsweise die Metadaten angeben, dass der **Typ** von Feuchtigkeitsdaten **int** ist, und **double** im Telemetriedatenstrom gefunden wird, werden die Telemetriedaten zur Feuchtigkeit nicht im Diagramm gezeigt. Allerdings werden die Werte für **Humidity** weiterhin gespeichert und für die Back-End-Verarbeitung zur Verfügung gestellt.

## Nächste Schritte

Nachdem Sie jetzt eine funktionsfähige vorkonfigurierte Lösung erstellt haben, können Sie mit den folgenden exemplarischen Vorgehensweisen fortfahren:

-   [Anleitung zum Anpassen vorkonfigurierter Lösungen][lnk-customize]
-   [Übersicht über die vorkonfigurierte Lösung für vorhersagbaren Wartungsbedarf][lnk-predictive]

[image1]: media/iot-suite-dynamic-telemetry/image1.png
[image2]: media/iot-suite-dynamic-telemetry/image2.png
[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-predictive]: iot-suite-predictive-overview.md
[lnk-node]: http://nodejs.org
[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdks

<!---HONumber=AcomDC_0615_2016-->