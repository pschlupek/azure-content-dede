<properties 
	pageTitle="Verwenden von Service Bus-Themen mit Python | Microsoft Azure" 
	description="Erfahren Sie mehr über die Verwendung von Azure Service Bus-Themen und -Abonnements über Python." 
	services="service-bus" 
	documentationCenter="python" 
	authors="sethmanheim" 
	manager="timlt" 
	editor=""/>

<tags 
	ms.service="service-bus" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="python" 
	ms.topic="article" 
	ms.date="05/06/2016" 
	ms.author="sethm"/>

# Verwenden von Service Bus-Themen und -Abonnements

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In diesem Artikel erfahren Sie, wie Sie Service Bus-Themen und -Abonnements verwenden. Die Beispiele sind in Python geschrieben und verwenden das [Python Azure-Paket][]. Die behandelten Szenarien umfassen das **Erstellen von Themen und Abonnements**, das **Erstellen von Abonnementfiltern**, das **Senden von Nachrichten an ein Thema**, das **Empfangen von Nachrichten von einem Abonnement** und das **Löschen von Themen und Abonnements**. Weitere Informationen zu Themen und Abonnements finden Sie im Abschnitt [Nächste Schritte](#next-steps).

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**Hinweis:** Informationen zur Installation von Python bzw. des [Python Azure-Pakets][] finden Sie in den [Installationsanweisungen für Python](../python-how-to-install.md).

## Erstellen eines Themas

Das **ServiceBusService**-Objekt ermöglicht Ihnen, mit Themen zu arbeiten. Fügen Sie am Anfang jeder Python-Datei, in der Sie programmgesteuert auf Service Bus zugreifen möchten, den folgenden Code hinzu:

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Der folgende Code erstellt ein **ServiceBusService**-Objekt. Ersetzen Sie `mynamespace`, `sharedaccesskeyname` und `sharedaccesskey` durch Ihren tatsächlichen Namespace, Shared Access Signature (SAS)-Schlüsselnamen und Schlüsselwert.

```
bus_service = ServiceBusService(
	service_namespace='mynamespace',
	shared_access_key_name='sharedaccesskeyname',
	shared_access_key_value='sharedaccesskey')
```

Sie können die Werte für den Namen und Wert des SAS-Schlüssels im [klassischen Azure-Portal][] im Fenster **Verbindungsinformationen** abrufen.

```
bus_service.create_topic('mytopic')
```

**create\_topic** unterstützt zudem weitere Optionen, mit denen Sie die Standardthemeneinstellungen überschreiben können, wie zum Beispiel die Nachrichtenlebensdauer oder maximale Themengröße. Das folgende Beispiel legt die maximale Themengröße auf 5 GB bei einer Gültigkeitsdauer (Time-to-live, TTL) von 1 Minute fest:

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## Erstellen von Abonnements

Abonnements von Themen werden ebenfalls mit dem **ServiceBusService**-Objekt erstellt. Abonnements werden benannt und können einen optionalen Filter aufweisen, der die Nachrichten einschränkt, die an die virtuelle Warteschlange des Abonnements übermittelt werden.

> [AZURE.NOTE] Abonnements sind dauerhaft und existieren solange, bis sie oder das Thema, für das sie abonniert sind, gelöscht werden.

### Erstellen eines Abonnements mit dem Standardfilter (MatchAll)

**MatchAll** ist der Standardfilter, der verwendet wird, wenn beim Erstellen eines neuen Abonnements kein Filter angegeben wird. Wenn der Filter **MatchAll** verwendet wird, werden alle für das Thema veröffentlichten Nachrichten in die virtuelle Warteschlange des Abonnements gestellt. Mit dem folgenden Beispiel wird ein Abonnement namens "AllMessages" erstellt, für das der Standardfilter **MatchAll** verwendet wird.

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### Erstellen von Abonnements mit Filtern

Sie können auch Filter definieren, durch die Sie angeben können, welche an ein Thema gesendeten Nachrichten in einem bestimmten Themenabonnement angezeigt werden sollen.

Der von Abonnements unterstützte flexibelste Filtertyp ist **SqlFilter**, der eine Teilmenge von SQL92 implementiert. SQL-Filter werden auf die Eigenschaften der Nachrichten angewendet, die für das Thema veröffentlicht werden. Weitere Informationen zu den Ausdrücken, die mit einem SQL-Filter verwendet werden können, finden Sie in der Syntax [SqlFilter.SqlExpression][].

Sie können einem Abonnement mithilfe der **create\_rule**-Methode des **ServiceBusService**-Objekts Filter hinzufügen. Durch diese Methode können Sie neue Filter zu einem vorhandenen Abonnement hinzufügen.

> [AZURE.NOTE] Da der Standardfilter automatisch auf alle neuen Abonnements angewendet wird, müssen Sie zuerst den Standardfilter entfernen. Ansonsten überschreibt **MatchAll** alle Filter, die Sie angeben. Sie können die Standardregel mithilfe der **delete\_rule**-Methode des **ServiceBusService**-Objekts entfernen.

Mit dem folgenden Beispiel wird ein Abonnement namens `HighMessages` mit einem **SqlFilter** erstellt, der nur Nachrichten auswählt, deren benutzerdefinierte **messagenumber**-Eigenschaft größer ist als 3:

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Ebenso erstellt das folgende Beispiel ein Abonnement namens `LowMessages` mit einem **SqlFilter**, der nur Nachrichten auswählt, deren benutzerdefinierte **messagenumber**-Eigenschaft kleiner oder gleich 3 ist:

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Wenn eine Nachricht an `mytopic` gesendet wird, wird diese nun stets an die Empfänger des Themenabonnements **AllMessages** zugestellt. Sie wird selektiv an die Empfänger der Themenabonnements **HighMessages** und **LowMessages** zugestellt (je nach Inhalt der Nachricht).

## Senden von Nachrichten an ein Thema

Um eine Nachricht an ein Service Bus-Thema zu senden, muss die Anwendung die **send\_topic\_message**-Methode des **ServiceBusService**-Objekts verwenden.

Das folgende Beispiel zeigt, wie Sie fünf Testnachrichten an `mytopic` senden. Beachten Sie, dass der **messagenumber**-Eigenschaftswert jeder Nachricht gemäß der Iteration der Schleife variiert (auf diese Weise wird bestimmt, welche Abonnements die Nachricht erhalten):

```
for i in range(5):
	msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
	bus_service.send_topic_message('mytopic', msg)
```

Service Bus-Themen unterstützen eine maximale Nachrichtengröße von 256 KB für den [Standard-Tarif](service-bus-premium-messaging.md) und 1 MB für den [Premium-Tarif](service-bus-premium-messaging.md). Der Header, der die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung für die Anzahl der Nachrichten, die ein Thema enthält. Es gibt jedoch eine Obergrenze für die Gesamtgröße der Nachrichten eines Themas. Die Themengröße wird bei der Erstellung definiert. Die Obergrenze beträgt 5 GB. Weitere Informationen zu Kontingenten finden Sie unter [Service Bus-Kontingente][].

## Empfangen von Nachrichten aus einem Abonnement

Nachrichten werden von einem Abonnement über die **receive\_subscription\_message**-Methode auf dem **ServiceBusService**-Objekt empfangen:

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Wenn der Parameter **peek\_lock** auf **False** festgelegt ist, werden Nachrichten nach dem Lesen aus dem Abonnement gelöscht. Sie können die Nachricht lesen (peek) und sperren, ohne sie aus der Warteschlange zu löschen, indem Sie den Parameter **peek\_lock** auf **True** festlegen.

Das Verhalten für das Lesen und Löschen der Nachricht als Teil des Empfangsvorgangs ist das einfachste Modell. Es wird am besten für Szenarien eingesetzt, bei denen es eine Anwendung tolerieren kann, wenn eine Nachricht bei Auftreten eines Fehlers nicht verarbeitet wird. Um dieses Verfahren zu verstehen, stellen Sie sich ein Szenario vor, in dem der Consumer die Empfangsanforderung ausstellt und dann abstürzt, bevor diese verarbeitet wird. Da Service Bus die Nachricht als verwendet markiert hat, wird er jene Nachricht auslassen, die vor dem Absturz verwendet wurde, wenn die Anwendung neu startet und erneut mit der Verwendung von Nachrichten beginnt.

Wenn der Parameter **peek\_lock** auf **True** festgelegt ist, wird der Empfangsvorgang zu einem zweistufigen Vorgang. Dadurch können Anwendungen unterstützt werden, die das Auslassen bzw. Fehlen von Nachrichten nicht zulassen können. Wenn Service Bus eine Anfrage erhält, ermittelt der Dienst die nächste zu verarbeitende Nachricht, sperrt diese, um zu verhindern, dass andere Consumer sie erhalten, und sendet sie dann zurück an die Anwendung. Nachdem die Anwendung die Verarbeitung der Nachricht abgeschlossen hat (oder sie zwecks zukünftiger Verarbeitung zuverlässig gespeichert hat), führt Sie die zweite Phase des Empfangsprozesses durch Aufrufen der **delete**-Methode des **Message**-Objekts aus. Die **delete**-Methode markiert die Nachricht als verarbeitet und entfernt sie aus dem Abonnement.

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## Behandeln von Anwendungsabstürzen und nicht lesbaren Nachrichten

Service Bus stellt Funktionen zur Verfügung, die Sie bei der ordnungsgemäßen Behandlung von Fehlern in der Anwendung oder bei Problemen beim Verarbeiten einer Nachricht unterstützen. Wenn eine Empfängeranwendung die Nachricht aus einem bestimmten Grund nicht verarbeiten kann, so kann sie die **unlock**-Methode zum **Message**-Objekt hinzufügen. Dies führt dazu, dass Service Bus die Nachricht innerhalb des Abonnements entsperrt und zur Verfügung stellt, damit sie erneut empfangen werden kann – durch die gleiche verarbeitende Anwendung oder durch eine andere verarbeitenden Anwendung.

Zudem wird einer im Abonnement gesperrten Anwendung ein Zeitlimit zugeordnet. Wenn die Anwendung die Nachricht vor Ablauf des Sperrzeitlimits nicht verarbeiten kann (zum Beispiel wenn die Anwendung abstürzt) entsperrt Service Bus die Nachricht automatisch und macht sie verfügbar, um erneut empfangen zu werden.

Falls die Anwendung nach der Verarbeitung der Nachricht, aber vor Abrufen der **delete**-Methode abstürzt, wird die Nachricht wieder an die Anwendung zugestellt, wenn diese neu gestartet wird. Dies wird häufig als **At Least Once Processing** (Verarbeitung mindestens einmal) bezeichnet und bedeutet, dass jede Nachricht mindestens einmal verarbeitet wird, wobei dieselbe Nachricht in bestimmten Situationen möglicherweise erneut zugestellt wird. Wenn eine doppelte Verarbeitung im betreffenden Szenario nicht geeignet ist, sollten Anwendungsentwickler ihrer Anwendung zusätzliche Logik für den Umgang mit der Übermittlung doppelter Nachrichten hinzufügen. Dies wird häufig durch die Verwendung der **MessageId**-Eigenschaft der Nachricht erzielt, die über mehrere Zustellungsversuche hinweg konstant bleibt.

## Löschen von Themen und Abonnements

Themen und Abonnements sind persistent und müssen über das [klassische Azure-Portal][] oder programmgesteuert explizit gelöscht werden. Im folgenden Beispiel wird das Thema `mytopic` gelöscht:

```
bus_service.delete_topic('mytopic')
```

Durch das Löschen eines Themas werden auch alle Abonnements gelöscht, die mit dem Thema registriert sind. Abonnements können auch unabhängig gelöscht werden. Der folgende Code zeigt, wie das Abonnement `HighMessages` aus dem Thema `mytopic` gelöscht wird:

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## Nächste Schritte

Nachdem Sie nun mit den Grundlagen der Service Bus-Themen vertraut sind, finden Sie unter den folgenden Links weitere Informationen.

-   Siehe [Service Bus-Warteschlangen, -Themen und -Abonnements][].
-   Referenz für [SqlFilter.SqlExpression][].

[klassische Azure-Portal]: http://manage.windowsazure.com
[klassischen Azure-Portal]: http://manage.windowsazure.com
[Python Azure-Paket]: https://pypi.python.org/pypi/azure
[Python Azure-Pakets]: https://pypi.python.org/pypi/azure
[Service Bus-Warteschlangen, -Themen und -Abonnements]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Service Bus-Kontingente]: service-bus-quotas.md

<!---HONumber=AcomDC_0525_2016-->