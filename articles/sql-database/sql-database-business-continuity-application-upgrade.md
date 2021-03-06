<properties 
   pageTitle="SQL-Datenbank-Geschäftskontinuität während Anwendungsupgrades" 
   description="Dieser Abschnitt enthält eine Anleitung zum Vermeiden von Ausfallzeiten während eines Anwendungsupgrades." 
   services="sql-database"
   documentationCenter="" 
   authors="carlrabeler" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="05/27/2016"
   ms.author="carlrab"/>

#Aktualisieren einer Anwendung ohne Ausfallzeiten

Im Zusammenhang mit Microsoft Azure bezieht sich der Begriff "Anwendung" auf Komponenten wie Front-Ends, Dienste, die in einem Clouddienst bereitgestellt werden, oder die Datenebene zum Beibehalten von Anwendungsdaten oder Metadaten. Cloudanwendungen sind häufig darauf ausgelegt, durchgehend ohne Unterbrechungen ausgeführt zu werden. Beim Einführen einer neuen Version der Anwendung kann es zu Störungen wie z. B. weniger verfügbaren Funktionen oder sogar vollständigen Ausfällen kommen, wenn Änderungen auf Datenebene auf die Live-Website übernommen werden.

Beim Entwerfen des Anwendungsaktualisierungsvorgangs sollte das wichtigste Ziel darin bestehen, die Zeit, während der der Funktionsumfang der Anwendung verringert ist, erheblich zu verkürzen oder Ausfälle ganz zu vermeiden. Um dies zu erreichen, schließt der Vorgang i. d. R. das Erstellen einer temporären Kopie der Anwendung ein, die als Sicherung verwendet wird, falls das Upgrade fehlschlägt. Beim Entwerfen und Planen der Aktualisierung sollten die folgenden Faktoren berücksichtigt werden:

1.	Maximale zulässige Zeit, während der der Funktionsumfang der Anwendung verringert ist 
2.	Mindestsatz an Funktionen, die während der Aktualisierung zur Verfügung stehen
3.	Möglichkeit zum Rollback bei Fehlern während des Upgrades
4.	Insgesamt anfallende Kosten. Dies schließt die Kosten von zusätzlichen erforderlichen Anwendungskomponenten zum Erstellen einer temporären Kopie (z.B. zusätzliche Datenbanken für aktive Georeplikation) und die Mehrkosten für temporäre Bereitstellungen während des Aktualisierungsvorgangs ein. 

Wenn die Anwendung vorübergehend im schreibgeschützten Modus betrieben werden kann, ist es möglich, den Aktualisierungsablauf so zu entwerfen, dass Ausfallzeiten effektiv vermieden werden können. Ausführliche Informationen zum Implementieren des Aktualisierungsablaufs für Ihre spezifische Anwendungstopologie finden Sie unter [Verwalten von parallelen Upgrades von Cloudanwendungen mithilfe der aktiven Georeplikation von SQL-Datenbank](sql-database-manage-application-rolling-upgrade.md).
 
 
 

<!---HONumber=AcomDC_0615_2016-->